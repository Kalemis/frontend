
# Async & Performance

## Table of contents
- [Asynchrony now and later](#asynchrony-now-and-later)  
- [Callbacks](#callbacks)  
- [Promises](#promises)  
- [Generators](#generators)  
- [Program performance](#program-performance)  
- [Benchmarking and tuning](#benchmarking-and-tuning)  
- [College notes](#college-notes)

## Asynchrony: now and later

### A program in chunks 
Your program will be compromised in several chunks, only one of them is executing now, the rest of the chunks comes later. The most common unit of chunk is function. 

Asynchroon voorbeeld: 
Je komt bij de Mac, besteld een burger. Vervolgens gaat de medewerker verder met helpen van andere klanten en jij moet wachten. Later word jij terug geroepen door de medewerker (callback), jouw burger is namelijk klaar. 
Js examples: timer, mouse click, Ajax response etc. 

### Parallel Threading
Async is about the gap between now and later. Parallel is about things being able to occur simultaneously. 

Parallel system with single thread behaviour will run line after line, without waiting for data. 
Parallel system where 2 different threads are operating in the same program, can give problems, because sometimes data isn’t ready yet. 

### Run-to-Completion
Because of JavaScript's single-threading, the code inside of foo() (and bar()) is atomic, which means that once foo() starts running, the entirety of its code will finish before any of the code in bar() can run, or vice versa. This is called "run-to-completion" behavior.

### Concurrency 
Concurrency is when two or more "processes" are executing simultaneously over the same period, regardless of whether their individual constituent operations happen in parallel (at the same instant on separate processors or cores) or not. You can think of concurrency then as "process"-level (or task-level) parallelism, as opposed to operation-level parallelism (separate-processor threads). 


### Noninteracting
As two or more "processes" are interleaving their steps/events concurrently within the same program, they don't necessarily need to interact with each other if the tasks are unrelated. If they don't interact, nondeterminism is perfectly acceptable.

### Interaction
More commonly, concurrent "processes" will by necessity interact, indirectly through scope and/or the DOM. When such interaction will occur, you need to coordinate these interactions to prevent "race conditions," as described earlier.

### Cooperation
Another expression of concurrency coordination is called "cooperative concurrency." Here, the focus isn't so much on interacting via value sharing in scopes (though that's obviously still allowed!). The goal is to take a long-running "process" and break it up into steps or batches so that other concurrent "processes" have a chance to interleave their operations into the event loop queue.

### Jobs

So, the best way to think about this that I've found is that the "Job queue" is a queue hanging off the end of every tick in the event loop queue. Certain async-implied actions that may occur during a tick of the event loop will not cause a whole new event to be added to the event loop queue, but will instead add an item (aka Job) to the end of the current tick's Job queue.
 
## Callbacks

Even though at an operational level our brains are async evented, we seem to plan out tasks in a sequential, synchronous way. "I need to go to the store, then buy some milk, then drop off my dry cleaning." So does the developer think: "I need to set z to the value of x, and then x to the value of y," and so forth.

Callbacks are the fundamental unit of asynchrony in JS. But they're not enough for the evolving landscape of async programming as JS matures.

First, our brains plan things out in sequential, blocking, single-threaded semantic ways, but callbacks express asynchronous flow in a rather nonlinear, nonsequential way, which makes reasoning properly about such code much harder. Bad to reason about code is bad code that leads to bad bugs. We need a way to express asynchrony in a more synchronous, sequential, blocking manner, just like our brains do.

Second, and more importantly, callbacks suffer from inversion of control in that they implicitly give control over to another party (often a third-party utility not in your control!) to invoke the continuation of your program. This control transfer leads us to a troubling list of trust issues, such as whether the callback is called more times than we expect.

If you have code that uses callbacks, especially but not exclusively with third-party utilities, and you're not already applying some sort of mitigation logic for all these inversion of control trust issues, your code has bugs in it right now even though they may not have bitten you yet. Latent bugs are still bugs.

### Inversion of control:

Inventing ad hoc logic to solve these trust issues is possible, but it's more difficult than it should be, and it produces clunkier and harder to maintain code, as well as code that is likely insufficiently protected from these hazards until you get visibly bitten by the bugs.

We need a generalized solution to all of the trust issues, one that can be reused for as many callbacks as we create without all the extra boilerplate overhead.

## Promises 

### What is a Promise ?
https://www.youtube.com/watch?v=swdWUWtGxR4

An individual Promise behaves as a future value. But there's another way to think of the resolution of a Promise: as a flow-control mechanism -- a temporal this-then-that -- for two or more steps in an asynchronous task.

In typical JavaScript fashion, if you need to listen for a notification, you'd likely think of that in terms of events. So we could reframe our need for notification as a need to listen for a completion (or continuation) event emitted by foo(..).

With callbacks, the "notification" would be our callback invoked by the task (foo(..)). But with Promises, we turn the relationship around, and expect that we can listen for an event from foo(..), and when notified, proceed accordingly.

### Promise trust 

What if we: 
• Call the callback too early
Promises by definition cannot be susceptible to this concern, because even an immediately fulfilled Promise (like new Promise(function(resolve){ resolve(42); })) cannot be observed synchronously.

That is, when you call then(..) on a Promise, even if that Promise was already resolved, the callback you provide to then(..) will always be called asynchronously

No more need to insert your own setTimeout(..,0) hacks. Promises prevent Zalgo automatically.

• Call the callback too late (or never)
Similar to the previous point, a Promise's then(..) registered observation callbacks are automatically scheduled when either resolve(..) or reject(..) are called by the Promise creation capability. Those scheduled callbacks will predictably be fired at the next asynchronous moment



• Call the callback too few or too many times
The "too many" case is easy to explain. Promises are defined so that they can only be resolved once. If for some reason the Promise creation code tries to call resolve(..) or reject(..) multiple times, or tries to call both, the Promise will accept only the first resolution, and will silently ignore any subsequent attempts.

• Fail to pass along any necessary environment/parameters
Promises can have, at most, one resolution value (fulfillment or rejection).

If you don't explicitly resolve with a value either way, the value is undefined, as is typical in JS. But whatever the value, it will always be passed to all registered (and appropriate: fulfillment or rejection) callbacks, either now or in the future.


• Swallow any errors/exceptions that may happen
In the base sense, this is a restatement of the previous point. If you reject a Promise with a reason (aka error message), that value is passed to the rejection callback(s).

But there's something much bigger at play here. If at any point in the creation of a Promise, or in the observation of its resolution, a JS exception error occurs, such as a TypeError or ReferenceError, that exception will be caught, and it will force the Promise in question to become rejected.


### Chain Flow 
Promises are not just a mechanism for a single-step this-then-that sort of operation. That's the building block, of course, but it turns out we can string multiple Promises together to represent a sequence of async steps.

The key to making this work is built on two behaviors intrinsic to Promises:

• Every time you call then(..) on a Promise, it creates and returns a new Promise, which we can chain with.
• Whatever value you return from the then(..) call's fulfillment callback (the first parameter) is automatically set as the fulfillment of the chained Promise (from the first point).

## Generators

**GEEN TENTAMENVRAAG**

## Program performance

## Benchmarking and tuning


# Async and Performance

## Table of contents
- [Asynchrony now and later](#asynchrony-now-and-later)  
- [Callbacks](#callbacks)  
- [Promises](#promises)  
- [Generators](#generators)  
- [Program performance](#program-performance)  
- [Benchmarking and tuning](#benchmarking-and-tuning)  
- [College notes](#college-notes)

## Asynchrony now and later

*A JavaScript program is (practically) always broken up into two or more chunks, where the first chunk runs now and the next chunk runs later, in response to an event. Even though the program is executed chunk-by-chunk, all of them share the same access to the program scope and state, so each modification to state is made on top of the previous state.*

You may write your JS program in one .js file, but your program is almost certainly comprised of several chunks, only one of which is going to execute now, and the rest of which will execute later. The most common unit of chunk is the ```function```.

The simplest way of "waiting" from *now* until *later* is to use a function, commonly called a callback function.

Any time you wrap a portion of code into a ```function``` and specify that it should be executed in response to some event (timer, mouse click, Ajax response, etc.), you are creating a *later* chunk of your code, and thus introducing asynchrony to your program.

*Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the **event loop** is a "tick." User interaction, IO, and timers enqueue events on the event queue.*

*At any given moment, only one event can be processed from the queue at a time. While an event is executing, it can directly or indirectly cause one or more subsequent events.*

***Concurrency** is when two or more chains of events interleave over time, such that from a high-level perspective, they appear to be running simultaneously (even though at any given moment only one event is being processed). Such as lazy loading: on-scroll events and Ajax repsonses run simultaneaously.*

```javascript 
var res = [];

function response(data) {
	res.push( data );
}

// ajax(..) is some arbitrary Ajax function given by a library
ajax( "http://some.url.1", response );
ajax( "http://some.url.2", response );
```
Either ```url.1``` can finish first or ```url.2```. You're not entirely sure which one will finish first: "race condition".

So, to address such a race condition, you can coordinate ordering interaction:
```javascript
var res = [];

function response(data) {
	if (data.url == "http://some.url.1") {
		res[0] = data;
	}
	else if (data.url == "http://some.url.2") {
		res[1] = data;
	}
}

// ajax(..) is some arbitrary Ajax function given by a library
ajax( "http://some.url.1", response );
ajax( "http://some.url.2", response );
```

*It's often necessary to do some form of interaction coordination between these concurrent "processes" (as distinct from operating system processes), for instance to ensure ordering or to prevent "race conditions." These "processes" can also cooperate by breaking themselves into smaller chunks and to allow other "process" interleaving.*


## Callbacks

*Callbacks are the fundamental unit of asynchrony in JS. But they're not enough for the evolving landscape of async programming as JS matures.*

*First, our brains plan things out in sequential, blocking, single-threaded semantic ways, but callbacks express asynchronous flow in a rather nonlinear, nonsequential way, which makes reasoning properly about such code much harder. Bad to reason about code is bad code that leads to bad bugs.
We need a way to express asynchrony in a more synchronous, sequential, blocking manner, just like our brains do.*

*Second, and more importantly, callbacks suffer from inversion of control in that they implicitly give control over to another party (often a third-party utility not in your control!) to invoke the continuation of your program. This control transfer leads us to a troubling list of trust issues, such as whether the callback is called more times than we expect.*

If you have code that uses callbacks, especially but not exclusively with third-party utilities, and you're not already applying some sort of mitigation logic for all these inversion of control trust issues, your code has bugs in it right now even though they may not have bitten you yet. Latent bugs are still bugs.

**Inversion of control:**

*Inventing ad hoc logic to solve these trust issues is possible, but it's more difficult than it should be, and it produces clunkier and harder to maintain code, as well as code that is likely insufficiently protected from these hazards until you get visibly bitten by the bugs.*

*We need a generalized solution to all of the trust issues, one that can be reused for as many callbacks as we create without all the extra boilerplate overhead.*

## Promises

## Generators

**GEEN TENTAMENVRAAG**

## Program performance

## Benchmarking and tuning

## College notes

**JSON:**  
JavaScript Object Notation. 

**JSON.parse:**  
Parse deze string naar echte JavaScript objecten.

**XMLHttpRequest:**  


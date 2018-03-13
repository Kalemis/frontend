# Table of Contents

- [This or that](#this-or-that)
- [This all makes sense now](#this-all-makes-sense-now)
- [Objects](#objects)
- [Mixing up class objects](#mixing-up-class-objects)
- [Prototypes](#prototypes)
- [Behavior delegation](#behavior-delegation)
- [ES6 Class](#ES6-class)
- [Thank You](#thank-you)

## This or that

## Why?
```This``` allows functions to be reused against multiple _context_ objects, rather than needing a seperate version of the function for each object.  
```This``` **DOES NOT** refer to a function's lexical scope.

```This``` is not an author-time binding but a runtime binding. It is contextual based on the conditions of the function's invocation. ```This``` binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which the function is called.

```This``` is actually a binding that is made when a function is invoked, and what it references is determined entirely by the call-site where the function is called.

## This all makes sense now
**Call site**: the location in code where a function is called (not where it's declared).  
**Call stack**: the stack of functions that have been called to get us to the current moment in execution.  
**Tip**: To see the call stack, go to the developer tools in the browser and set a breakpoint on the function. The tool will show you a list of functions that have been called to get to that line, which will be your call stack. Now find the second item from the top, that will show you the real call-site.  

###

## Objects

This is an h3 heading

## Mixing up class objects

This is an h1 heading

## Prototypes

This is an h2 heading

## Behavior delegation

## ES6 Class

This is an h3 heading

## Thank You

This is an h1 heading


This is an h3 heading

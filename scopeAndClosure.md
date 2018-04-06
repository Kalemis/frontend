# Scope and closure

## Table of contents
- [Lexical scope](#lexical-scope)
- [Function vs Block Scope](#function-vs-block-scope)
- [Hoisting](#hoisting)
- [Scope closures](#scope-closures)
- [College Notes](#college-notes)

## Lexical scope

### Scope  
The set of rules that govern how the *Engine* can look up a variable by its identifier name and find it, either in the current *Scope*, or in any of the *Nested scopes* it's contained within. 

There are 2 models for how scope works: 
- Lexical Scope
- Dynamic Scope

### Lexical Scope
Lexical scope is scope that is defined at **lexing** time.  
**Lexing**: The first traditional phase of a standard language compiler is called lexing.  
So, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is set in stone by the time the lexer processes your code.

```javascript
function foo(a) {

	var b = a * 2;

	function bar(c) {
		console.log( a, b, c );
	}

	bar(b * 3);
}

foo( 2 ); // 2 4 12
```
In the example above console.log will first find ```c``` than ```b``` and than ```a```. But if there had been a ```c``` both in bar and in foo, it would have stopped looking for ```c``` in bar because it had already found it.  
**Scope look-up starts at the innermost scope being executed at the time, and works its way outward/upward until it finds the first match.**

### Cheating the lexical scope
There are 2 ways to cheat the lexical scope (both modify at runtime): 
- ```eval(..)```
- ```with```

#### ```eval(..)```
```eval(..)``` takes a string as an argument, and treats the contents of the string as if it had actually been authored code at that point in the program. So, you can programmatically generate code inside of your authored code, and run the generated code as if it had been there at author time.

### ```with```
What is ```with```?  
Short-hand for making multiple property references against an object *without* repeating the object reference itself each time.

How to cheat lexical scope with ```with```:
The with statement takes an object, one which has zero or more properties, and treats that object as if it is a wholly separate lexical scope, and thus the object's properties are treated as lexically defined identifiers in that "scope".

### Why you shouldn't use them
The downside to these mechanisms is that it defeats the Engine's ability to perform compile-time optimizations regarding scope look-up, because the Engine has to assume pessimistically that such optimizations will be invalid. Code will run slower as a result of using either feature. Don't use them.

## Function vs Block Scope

### Scope from functions
Each function you declare creates its own scope. Variables and functions that are declared inside another function are essentially "hidden" from any of the enclosing "scopes", which is an intentional design principle of good software.

### Hiding in plain scope
"Hiding" variables and fuctions: 
Instead of declaring a function and than adding code inside it, you can also think of it as the other way around. So, take a section of code you've written and wrap a function declaration around it which "hides" the code. Now you've created a scope around the code in question.  
*This is considered better software: principle of least privilege*

Another benefit of "hiding" variables and functions inside a scope is to avoid unintended collision between two different identifiers with the same name but different intended usages.
Another option for collision avoidance is the more modern "module" approach, using any of various dependency managers. (this is a tool)

### Functions as Scopes

Compare the following to code snippets.
``` javascript 
function foo() {
//some code
};
```
```javascript
(function foo(){
//some code
});
```
In the first code snippet foo is polluting the global scope. In the second code snippet foo is only accesible inside itself. The function is here treated as a function expression.

#### Anonymous vs named
Only function expressions can be anonymous. But it's better to name it anyway.
Function declarations cannot be anonymous, it would be illegal JS grammar.

#### Invoking function expressions immediately (IFFE)
Now that we have a function as an expression by virtue of wrapping it in a ```( )``` pair, we can execute that function by adding another ```()``` on the end, like ```(function foo(){ .. })()```. The first enclosing ```( )``` pair makes the function an expression, and the second ```()``` executes the function.

A variation on the IFFE looks like this: ```(function(){ .. }())```. Which you use is a pure stylistic preference. They work either way.

### Blocks as scopes
Block scope is all about declaring variables as close as possible, as local as possible, to where they will be used.  
Block-scope refers to the idea that variables and functions can belong to an arbitrary block (generally, any { .. } pair) of code, rather than only to the enclosing function.

#### ```let```
The ```let``` keyword attaches the variable declaration to the scope of whatever block (commonly a ```{ .. }``` pair) it's contained in. In other words, let implicitly hijacks any block's scope for its variable declaration.

Declarations made with let will not hoist to the entire scope of the block they appear in

##### ```let``` loops
``` javascript
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```
Not only does let in the for-loop header bind the i to the for-loop body, but in fact, it re-binds it to each iteration of the loop, making sure to re-assign it the value from the end of the previous loop iteration.

#### ```const```
Creates a block-scoped variable, but whose value is fixed (constant). Any attempt to change that value at a later time results in an error.

## Hoisting

``` javascript
a = 2;

var a;

console.log(a); //output: 2
```

``` javascript
console.log(a); //output: undefined
var a = 2;
```

So, what is going on here?
When you see ```var a = 2;```, you probably think of that as one statement. But JavaScript actually thinks of it as two statements: ```var a;``` and ```a = 2;```. The first statement, the declaration, is processed during the compilation phase. The second statement, the assignment, is left in place for the execution phase. **Thus, the declaration first and then the assignment**. 

The *Engine* handles the first code snippet like this: 
``` javascript
var a;
```
``` javascript
a = 2;
console.log(a);
```

The second code snippet like this: 
``` javascript
var a;
```
``` javascript
console.log(a);
a = 2;
```
So, the declarations are "moved" to the top, they are *hoisted*. But, they are not hoisted to the top of the program but to the top of the scope!

### Functions first
Functions are hoisted first and then variables.

While multiple/duplicate ```var``` declarations are effectively ignored, subsequent function declarations do override previous ones. It's probably best to avoid declaring functions in blocks.

## Scope closures

## College Notes

### Closure
```javascript
setTimeout(inner, 1000)
```
Dit is een callback. Net als
```javascript
body.getElementById(click(), inner)
```
Dit zie je omdat de haakjes er nog niet bij staan, die haakjes worden geregeld door click of setTimeout.

### Hoisting

``` javascript
myLittleFunction();
var myLittleFunction = function() {};
```
Hier krijg je een error: myLittleFunction is not a function. Als je een functie aanroept die niet bestaat krijg je deze error. Bij variabelen krijg je een reference error. 

``` javascript
myLittleFunction();
function myLittleFunction() {
  console.log('Am I hoisted?')
};
```

Nu gaat de code wel goed.

**Hoisting:** je variabele en functie declaraties die je doet worden naar de top van de functie getakeld/gehoist. Ze worden eerst gedeclareerd, ookal heb je ze pas later neergezet.

### Iffe


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
**Scope look-up stops once it finds the first match.**


## Function vs Block Scope

## Hoisting

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


# Scope and closure

## Table of contents
- [Lexical scope](#lexical-scope)
- [Function vs Block Scope](#function-vs-block-scope)
- [Hoisting](#hoisting)
- [Scope closures](#scope-closures)
- [College Notes](#college-notes)

## Lexical scope

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


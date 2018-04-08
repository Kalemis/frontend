# Table of Contents

- [This or that](#this-or-that)
- [This all makes sense now](#this-all-makes-sense-now)
- [Objects](#objects)
- [Mixing up class objects](#mixing-up-class-objects)
- [Prototypes](#prototypes)
- [Behavior delegation](#behavior-delegation)
- [ES6 Class](#ES6-class)
- [Thank You](#thank-you)
- [College Notes](#college-notes)

## This or that

## Why?
```This``` allows functions to be reused against multiple _context_ objects, rather than needing a seperate version of the function for each object.  
```This``` **DOES NOT** refer to a function's lexical scope.

```This``` is not an author-time binding but a runtime binding. It is contextual based on the conditions of the function's invocation. ```This``` binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which the function is called.

```This``` is actually a binding that is made when a function is invoked, and what it references is determined entirely by the call-site where the function is called.

## This all makes sense now

[Watch this video!](https://www.youtube.com/watch?v=zE9iro4r918)

**Call site**: the location in code where a function is called (not where it's declared).  
**Call stack**: the stack of functions that have been called to get us to the current moment in execution.  
**Tip**: To see the call stack, go to the developer tools in the browser and set a breakpoint on the function. The tool will show you a list of functions that have been called to get to that line, which will be your call stack. Now find the second item from the top, that will show you the real call-site.  

Where does ```this``` refer to?
You have to ask yourself: where was the function invoked(aangeroepen)?
4 rules: 
- implicit binding
- explicit binding
- new binding
- window binding

### Implicit binding
*Most common*
When the function is called, look to the left of the dot: that is where ```this``` is referring to. 

``` javascript 
var me = {
  name: 'Tyler', 
  age: 25,
  sayName: function(){
    console.log(this.name);
  }
};

me.sayName(); //this refers to me.
```

### Explicit binding
With the keywords ```.call```, ```.apply``` and ```.bind``` you explicitly tell what ```this``` is. 
```javascript 
var sayName = function(){
  console.log('My name is ' + this.name);
};

var stacey = {
  name: 'Stacey', 
  age: 34
};

sayName.call(stacey);
```

### New binding
The new binding rule states that when a function is invoked with the ```new``` keyword, the ```this``` keyword inside that function is bound to the new object being constructed. 

### Window binding
If you invoke a function that uses the ```this``` keyword, but doesn't have anything left to the ```.```, it's not using the ```new``` binding and you're not using ```call```, ```apply``` or ```bind```, then the ```this``` keyword is going to default to the window object. 

In strict mode this gives an error, if you're not in strict mode you get ```undefined```.
This is the default rule, when none of the above apply. 

### arrow functions
Instead of the four standard binding rules, ES6 arrow-functions use lexical scoping for this binding, which means they adopt the this binding (whatever it is) from its enclosing function call. They are essentially a syntactic replacement of self = this in pre-ES6 coding.

## Objects

[Watch this video about Objects!](https://www.youtube.com/watch?v=mgwiCUpuCxA)

Een object is een stukje data dat **properties** en **methods** hebben.
Properties: een clown heeft een naam, een schoen een maat. (key/value pairs)
Methods: een clown kan lachen/huilen (acties die een clown kan uitvoeren)

### Two forms
- declarative (literal) form
- constructed form 

**literal:**
``` javascript 
var myObj = {
	key: value
	// ...
};
```

**constructed:**
``` javascript 
var myObj = new Object();
myObj.key = value;
```

The only difference really is that you can add one or more key/value pairs to the literal declaration, whereas with constructed-form objects, you must add the properties one-by-one.

### Types
6 primary types:
- ```string```
- ```number```
- ```boolean```
- ```null```
- ```undefined```
- ```object```

Not everyting is an ```object```. There are several object sub-types (built-in objects).
- ```String```
- ```Number```
- ```Boolean```
- ```Object```
- ```Function```
- ```Array```
- ```Date```
- ```RegExp```
- ```Error```
These are built-in functions and each can be used as a constructor (with the ```new``` operator), with the result being a newly constructed object of the sub-type in question. 

Objects are collections of key/value pairs. The values can be accessed as properties, via .propName or ["propName"] syntax. Whenever a property is accessed, the engine actually invokes the internal default [[Get]] operation (and [[Put]] for setting values), which not only looks for the property directly on the object, but which will traverse the [[Prototype]] chain (see Chapter 5) if not found.

Properties have certain characteristics that can be controlled through property descriptors, such as writable and configurable. In addition, objects can have their mutability (and that of their properties) controlled to various levels of immutability using Object.preventExtensions(..), Object.seal(..), and Object.freeze(..).

Properties don't have to contain values -- they can be "accessor properties" as well, with getters/setters. They can also be either enumerable or not, which controls if they show up in for..in loop iterations, for instance.

You can also iterate over the values in data structures (arrays, objects, etc) using the ES6 for..of syntax, which looks for either a built-in or custom @@iterator object consisting of a next() method to advance through the data values one at a time.

## Mixing up class objects

Classes are a design pattern. Many languages provide syntax which enables natural class-oriented software design. JS also has a similar syntax, but it behaves very differently from what you're used to with classes in those other languages.

Classes mean copies.

When traditional classes are instantiated, a copy of behavior from class to instance occurs. When classes are inherited, a copy of behavior from parent to child also occurs.

Polymorphism (having different functions at multiple levels of an inheritance chain with the same name) may seem like it implies a referential relative link from child back to parent, but it's still just a result of copy behavior.

JavaScript does not automatically create copies (as classes imply) between objects.

The mixin pattern (both explicit and implicit) is often used to sort of emulate class copy behavior, but this usually leads to ugly and brittle syntax like explicit pseudo-polymorphism (OtherObj.methodName.call(this, ...)), which often results in harder to understand and maintain code.

Explicit mixins are also not exactly the same as class copy, since objects (and functions!) only have shared references duplicated, not the objects/functions duplicated themselves. Not paying attention to such nuance is the source of a variety of gotchas.

In general, faking classes in JS often sets more landmines for future coding than solving present real problems.

## Prototypes
[To understand classes and prototypes - watch this!](https://www.youtube.com/watch?v=sWOXYDBbz0g)

``` javascript 
var Person = function(name){
  this.name = name;
}; //This is our class

Person.prototype.sayName() = function(){
  console.log("Hi my name is " + this.name)
}
// Now both john and bobby can access sayName. You can use .prototype because it is created for you when you created the constructor. 

var john = new Person("john"); //This is a creation
var bobby = new Person("bobby"); //This is a creation

console.log(john.name); //john
bobby.sayName(); //Hi my name is bobby
```
Here ```Person``` is capitalized because it's a constructor. When you create a constructor it creates something that's called a Prototype for you. 

## Behavior delegation
Classes and inheritance are a design pattern you can choose, or not choose, in your software architecture. Most developers take for granted that classes are the only (proper) way to organize code, but here we've seen there's another less-commonly talked about pattern that's actually quite powerful: behavior delegation.

Behavior delegation suggests objects as peers of each other, which delegate amongst themselves, rather than parent and child class relationships. JavaScript's [[Prototype]] mechanism is, by its very designed nature, a behavior delegation mechanism. That means we can either choose to struggle to implement class mechanics on top of JS (see Chapters 4 and 5), or we can just embrace the natural state of [[Prototype]] as a delegation mechanism.

When you design code with objects only, not only does it simplify the syntax you use, but it can actually lead to simpler code architecture design.

OLOO (objects-linked-to-other-objects) is a code style which creates and relates objects directly without the abstraction of classes. OLOO quite naturally implements [[Prototype]]-based behavior delegation.

## College Notes

### This
- Runtime binding. 
- Verwijst naar de context waar je op het moment in zit, niet naar de scope.

``` javascript
var clowns = new Array() // array literal (met new maak je een object aan)
var clown = {
  name: 'Pipo', //name: key - pipo: value
  shoeSize: 80,
  laugh: function(){
    console.log('Ik heet ' + clown.name + ' hahahaha')
    console.log('Ik heet ' + this.name + ' hahahaha')
  }
} // object literal
```

Een object kan **properties** of **methods** hebben.
Properties: een clown heeft een naam, een schoen een maat. (key/value pairs)
Methods: een clown kan lachen/huilen (acties die een clown kan uitvoeren)

**Dot notation:** methods en properties aanroepen. Zoals ```clown.name``` of ```this.name```.  
**This:** This wordt in de scope van de functie automatisch gegenereerd en het verwijst naar het object waar je op dat moment in zit, oftewel de context.

### Constructor functie
Alleen *constructor* functies beginnen met een hoofdletter.
Soort blauwdruk waar je aan de hand daarvan objecten kan maken.




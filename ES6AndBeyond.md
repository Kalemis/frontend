# ES6 & Beyond

## Chapter 1: ES? Now & Future

### Introduction

It's time we dive into the evolution of JS to explore all the changes coming not only soon but farther over the horizon.
Unlike ES5, ES6 is not just a modest set of new APIs added to the language. It incorporates a whole slew of new syntactic forms, some of which may take quite a bit of getting used to. There's also a variety of new organization forms and new API helpers for various data types.
ES6 is a radical jump forward for the language. Even if you think you know JS in ES5, ES6 is full of new stuff you don't know yet, so get ready! This book explores all the major themes of ES6 that you need to get up to speed on, and even gives you a glimpse of future features coming down the track that you should be aware of.
Warning: All code in this book assumes an ES6+ environment. At the time of this writing, ES6 support varies quite a bit in browsers and JS environments

### Versioning

The JavaScript standard is referred to officially as "ECMAScript" - ES. "5" for "5th edition".
The earliest versions, ES1 and ES2, were not widely known or implemented. 
ES3 was the first widespread baseline for JavaScript
For political reasons beyond what we'll cover here, the ill-fated ES4 never came about.
In 2009, ES5 was officially finalized (later ES5.1 in 2011), and and explosion of browsers, such as Firefox, Chrome, Opera, Safari, and many others.
Leading up to the expected next version of JS (slipped from 2013 to 2014 and then 2015), the obvious and common label in discourse has been ES6.
suggestions have surfaced that versioning may in the future switch to a year-based schema, such as ES2016
It's also valid to consider the future of JS versioning to be per-feature rather than per-arbitrary-collection-of-major-features (as it is now) or even per-year (as it may become).
The takeaway is that the version labels stop being as important, and JavaScript starts to be seen more as an evergreen, living standard.

### Transpiling

A problem arises for JS developers who at once may both strongly desire to use new features while at the same time being slapped with the reality that their sites/apps may need to support older browsers without such support.
many are just recently (at the time of this writing) starting to adopt things like ``` strict ``` mode, which landed in ES5 over five years ago.
So how do we resolve this seeming contradiction? The answer is tooling, specifically a technique called transpiling (transformation + compiling)
Transpilers perform transformations for you, usually in a build workflow step similar to how you perform linting, minification, and other similar operations.

### Shims/Polyfills

Not all new ES6 features need a transpiler. Polyfills (aka shims) are a pattern for defining equivalent behavior from a newer environment into an older environment, when possible. Syntax cannot be polyfilled, but APIs often can be.
SHIM : For instance the es5-shim npm package will let you write ECMAScript 5 (ES5) syntax and not care if the browser is running ES5 or not. Take Date.now as an example. This is a new function in ES5 where the syntax in ES3 would be new Date().getTime(). If you use the es5-shim you can write Date.now and if the browser you’re running in supports ES5 it will just run. However, if the browser is running the ES3 engine es5-shim will intercept the call to Date.now and just return new Date().getTime() instead. 
Polyfilss : A polyfill is a piece of code (or plugin) that provides the technology that you, the developer, expect the browser to provide natively. You typically check if a browser supports a given API and load a polyfill if it doesn’t.

### Conslusion
It is assumed that JS will continue to evolve constantly, with browsers rolling out support for features continually rather than in large chunks. So the best strategy for keeping updated as it evolves is to just introduce polyfill shims into your code base, and a transpiler step into your build workflow, right now and get used to that new reality

## Chapter 2: Syntax


### Introduction

ES6 adds quite a few new syntactic forms that take some getting used to. In this chapter, we'll tour through them to find out what's in store.

### Block-scoped declarations

 the fundamental unit of variable scoping in JavaScript has always been the function
However, we can now create declarations that are bound to any block, called (unsurprisingly) block scoping. This means all we need is a pair of { .. } to create a scope
Hierin kun je dan een let of een var gebruiken. Bij het gebruiken van een var in een block-scoped kun deze ook buiten dit block gebruik. Als je een let gebruikt kun je hier echter niet bij buiten de scope. Op deze manier kun je dus een value opslaan in een let zonder dat deze zich in een functie staan, maar kan je er vanaf buiten alsnog niet bij. Een block-scoped wordt dan ook altijd in combinatie gebruikt met een let omdat de block-scoped anders geen enkele waarde heeft. Het mag echter wel los van elkaar gebruikt worden.

VOORBEELD 1:


var a = 2;

{
	let a = 3;
	console.log( a );	// 3
}

console.log( a );		// 2


VOORBEELD 2:

let x = 1;

if(x === 1){
	let x = 2;
	console.log(x);		//2
}

console.log(x);			//1


Accessing a let-declared variable earlier than its let .. declaration/initialization causes an error, whereas with vardeclarations the ordering doesn't matter (except stylistically).

{
	console.log( a );	// undefined
	console.log( b );	// ReferenceError!

	var a;
	let b;
}

Als je dus een block hebt met daarin let’s moet je deze altijd boven aan zetten omdat een dudielijker overzicht voor andere codeerders die naar jou code kijken en omdat je zo al veel errors voorkomt. Een let wordt dan ook niet gehoist

There's one other form of block-scoped declaration to consider: the const, which creates constants.
What exactly is a constant? It's a variable that's read-only after its initial value is set. You are not allowed to change the value the variable holds once it's been set
Consider:

{
	const a = 2;
	console.log( a );	// 2

	a = 3;				// TypeError!
}

Constants are not a restriction on the value itself, but on the variable's assignment of that value. In other words, the value is not frozen or immutable because of const, just the assignment of it. If the value is complex, such as an object or array, the contents of the value can still be modified:

{
	const a = [1,2,3];
	a.push( 4 );
	console.log( a );		// [1,2,3,4]

	a = 42;					// TypeError!
}

const can be used with variable declarations of for, for..in, and for..of loops (see "for..of Loops"). However, an error will be thrown if there's any attempt to reassign, such as the typical i++ clause of a for loop.

It's not really a protection, as many believe, because any later developer who wants to change a value of a const can just blindly change const to let on the declaration. 
My advice: to avoid potentially confusing code, only use const for variables that you're intentionally and obviously signaling will not change.

### Spread/rest
ES6 introduces a new ... operator that's typically referred to as the spread or rest operator, depending on where/how it's used. Let's take a look:

function foo(x,y,z) {
	console.log( x, y, z );
}

foo( ...[1,2,3] );				// 1 2 3

When ... is used in front of an array (actually, any iterable, which we cover in Chapter 3), it acts to "spread" it out into its individual values.
You'll typically see that usage as is shown in that previous snippet, when spreading out an array as a set of arguments to a function call
But ... can be used to spread out/expand a value in other contexts as well, such as inside another array declaration:

var a = [2,3,4];
var b = [ 1, ...a, 5 ];

console.log( b );					// [1,2,3,4,5]

The other common usage of ... can be seen as essentially the opposite; instead of spreading a value out, the ...gathers a set of values together into an array. Consider:

function foo(x, y, ...z) {
	console.log( x, y, z );
}

foo( 1, 2, 3, 4, 5 );			// 1 2 [3,4,5]

The ...z in this snippet is essentially saying: "gather the rest of the arguments (if any) into an array called z." Because x was assigned 1, and y was assigned 2, the rest of the arguments 3, 4, and 5 were gathered into z.
Of course, if you don't have any named parameters, the ... gathers all arguments:

function foo(...args) {
	console.log( args );
}

foo( 1, 2, 3, 4, 5);			// [1,2,3,4,5]

The ...args in the foo(..) function declaration is usually called "rest parameters," because you're collecting the rest of the parameters.

## Symbols

Here's how you create a symbol:

var sym = Symbol( "some optional description" );
typeof sym;		// "symbol"

Some things to note:
* You cannot and should not use new with Symbol(..). It's not a constructor, nor are you producing an object.
* The parameter passed to Symbol(..) is optional. If passed, it should be a string that gives a friendly description for the symbol's purpose.
* The typeof output is a new value ("symbol") that is the primary way to identify a symbol.
The main point of a symbol is to create a string-like value that can't collide with any other value. So, for example, consider using a symbol as a constant representing an event name:
const EVT_LOGIN = Symbol( "event.login" );

You'd then use EVT_LOGIN in place of a generic string literal like "event.login":

evthub.listen( EVT_LOGIN, function(data){
	// ..
} );
The benefit here is that EVT_LOGIN holds a value that cannot be duplicated (accidentally or otherwise) by any other value, so it is impossible for there to be any confusion of which event is being dispatched or handled.

You may use a symbol directly as a property name/key in an object, such as a special property that you want to treat as hidden or meta in usage. It's important to know that although you intend to treat it as such, it is not actually a hidden or untouchable property.

One mild downside to using symbols as in the last few examples is that the EVT_LOGIN and INSTANCE variables had to be stored in an outer scope (perhaps even the global scope), or otherwise somehow stored in a publicly available location, so that all parts of the code that need to use the symbols can access them.

Symbol.for(..) looks in the global symbol registry to see if a symbol is already stored with the provided description text, and returns it if so. If not, it creates one to return. In other words, the global symbol registry treats symbol values, by description text, as singletons themselves.
But that also means that any part of your application can retrieve the symbol from the registry using Symbol.for(..), as long as the matching description name is used.

If a symbol is used as a property/key of an object, it's stored in a special way so that the property will not show up in a normal enumeration of the object's properties:

Object.getOwnPropertySymbols( o );	// [ Symbol(bar) ]


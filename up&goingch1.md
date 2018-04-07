# Up&going ch1

## The basics

### Statements
In a computer language, a group of words, numbers, and operators that performs a specific task is a statement. In JavaScript, a statement might look as follows:

`a = b * 2;`

>The characters a and b are called variables (see "Variables"), which are like simple boxes you can store any of your stuff in.
>"2" is just a value, called a literal value. The `=` and `*` characters are operators (see "Operators") -- they perform actions with the values and variables such as assignment and mathematic multiplication.

### expressions

Statements are made up of one or more expressions. An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

`a = b * 2;`

- 2 is a literal value expression
- b is a variable expression, which means to retrieve its current value
- b * 2 is an arithmetic expression, which means to do the multiplication
- a = b * 2 is an assignment expression, which means to assign the result of the b * 2 expression to the variable a (more on assignments later)

### Values and types
~~~
"I am a string";  //a string

42; //a number

true; //boolean
false; //boolean
~~~

### Converting between types (coercion)
~~~
var a = "42";
var b = Number( a );

console.log( a );	// "42"
console.log( b );	// 42
~~~

> Here you make var `a`, a number.

### loops

While and do while loop

~~~
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
~~~

For loop
~~~
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
~~~

### function

A function is generally a named section of code that can be "called" by name, and the code inside it will be run each time. Consider:

~~~
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
~~~

### Scope

Programming has a term for this concept: scope (technically called lexical scope). In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's scoped variables.
~~~
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
~~~

### Review

Learning programming doesn't have to be a complex and overwhelming process. There are just a few basic concepts you need to wrap your head around.

These act like building blocks. To build a tall tower, you start first by putting block on top of block on top of block. The same goes with programming. Here are some of the essential programming building blocks:

- You need operators to perform actions on values.
- You need values and types to perform different kinds of actions like math on numbers or output with strings.
- You need variables to store data (aka state) during your program's execution.
- You need conditionals like if statements to make decisions.
- You need loops to repeat tasks until a condition stops being true.
- You need functions to organize your code into logical and reusable chunks.
- Code comments are one effective way to write more readable code, which makes your program easier to understand, maintain, and fix later if there are problems.

Finally, don't neglect the power of practice. The best way to learn how to write code is to write code.

I'm excited you're well on your way to learning how to code, now! Keep it up. Don't forget to check out other beginner programming resources (books, blogs, online training, etc.). This chapter and this book are a great start, but they're just a brief introduction.

The next chapter will review many of the concepts from this chapter, but from a more JavaScript-specific perspective, which will highlight most of the major topics that are addressed in deeper detail throughout the rest of the series.

# Up&going ch2

In de console heb je de type of operator die je verteld war de value is van iets.
Var a = 42
Type of a = number

Objects	Meerdere properties met hun eigen value van verschillende soorten bij elkaar zetten

Arrays	Slaat meerdere properties met hun eigen value van elke soort type op in numerieke volgorde

Voorbeelden built-in type methodes
Var a = ‘hello world’
Var b = 3.14125

a.length;		11
a.toUpperCase();	“HELLO WORLD”
b.toFixed(4);		3.1416

Comparing values	Komt altijd een boolean uit (treu/false)


coercian	var a = “42”			// “42”
		var b = number(a)		// 42


Truthly				Falsy
“x”				“ “
42				0, -0, NaN
true				null, undefined
[ ], [1,2,3]			false
{ }, {a:42}
function fo() { }

Variable namen moeten valide identifiers zijn, dat zijn ze als ze starten met a-z, A-Z, $, _

Hoisting	Als de variable boven de scope worden benoemd

Nested scopes	Als je een variable in een scope benoemd is deze in de hele scope te gebruiken, ook in de lower/inner scopes. 			Maar de inner niet in die daarbuiten

LET	Door let te gebruiken inplaats van var wordt het alleen op die plek bruikbaar in plaats van door heel dat block of de 		functie

Inplaats van if, else if, else if, else kun je ook een switch statement gebruiken

Switch(a){

	Case a;
	// do something

	Defautl;
	// fall back to here

}


Wat je ook kunt doen inplaats van een if,else

var a = 42;
var b = (a>41)?”Hello”:”World”;

Strict mode	Zorgt ervoor dat je betere ‘seare’ javascript schrijft. Omdat het bad syntax into real errors. “use strict” zet je bijvoorbeeld boven in een functie of helemaal boven in je code.

IIFE	Immediately Invoked Function Expressions. Door de code als een IIFE te schrijven wordt deze gelijk gedefineerd en gelijk uitgevoerd

( funtion(){
			…
})();

De eerste ()	Deze functie moet anders behandelt worden
De tweede ()	De functie moet gelijk uitgevoerd worden

Closure	Onthoud de variable die in een scope zitten pok nadat de functie is afgelopen
		Make Adder		Zijn hier ook een manier voor
		Modules		Zijn hier ook een manier voor
Old & New JavaScript -> 2 main techniques
1.	Polyfilling	
Een nieuwe functie laten werken door soortgelijk code, maar die werkt wel in oudere browsers.
2.	Transpilling
Een tool die jouw nieuwe code verandert naar oude code

Non-Javascript	Niet in directe controle van JS
			DOM
			Alert
			Console.log


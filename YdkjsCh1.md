# YDKJS ch1

### What is scope?

One of the most fundamental paradigms of nearly all programming languages is the ability to store values in variables, and later retrieve or modify those values. But the inclusion of variables into our program begets the most interesting questions we will now address: where do those variables live? In other words, where are they stored? And, most importantly, how does our program find them when it needs them?

### Compiler theory

Javascript valt onder de "dynamic" en "interpreted" programmeer talen. Het is een gecompileerde taal. Het wordt niet van te voren gecompileerd zoals veel gecompileerde talen en de resultaten van de compilatie zijn niet mee te nemen naar andere besturingssystemen of "enviroments".

### Compilation process:

- __Tokenizing/lexing__: breaking up a string of characters into meaningful (to the language) chunks, called tokens. For instance, consider the program: var a = 2;. This program would likely be broken up into the following tokens: var, a, =, 2, and ;. Whitespace may or may not be persisted as a token, depending on whether it's meaningful or not.

- __Parising__: taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an __"AST"__ (Abstract Syntax Tree).
The tree for var a = 2; might start with a top-level node called VariableDeclaration (__de "var"__), with a child node called Identifier (__de "a"__), and another child called AssignmentExpression (__de "="__) which itself has a child called NumericLiteral (__de "2"__).

- __Code-generation__: Hierin wordt de __"AST"__ omgezet in uitvoerbare code.

### lhs/rhs

(quick disclaimer, volgens mij is dit samengevat wat het betekend.)

- lhs lookup is a container lookup
- rhs lookup is a value lookup

### Engine/Scope Conversation (handig om door te nemen)
~~~
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
~~~

>Engine: Hey Scope, I have an RHS reference for foo. Ever heard of it?

>Scope: Why yes, I have. Compiler declared it just a second ago. He's a function. Here you go.

>Engine: Great, thanks! OK, I'm executing foo.

>Engine: Hey, Scope, I've got an LHS reference for a, ever heard of it?

>Scope: Why yes, I have. Compiler declared it as a formal parameter to foo just recently. Here you go.

>Engine: Helpful as always, Scope. Thanks again. Now, time to assign 2 to a.

>Engine: Hey, Scope, sorry to bother you again. I need an RHS look-up for console. Ever heard of it?

>Scope: No problem, Engine, this is what I do all day. Yes, I've got console. He's built-in. Here ya go.

>Engine: Perfect. Looking up log(..). OK, great, it's a function.

>Engine: Yo, Scope. Can you help me out with an RHS reference to a. I think I remember it, but just want to double-check.

>Scope: You're right, Engine. Same guy, hasn't changed. Here ya go.

>Engine: Cool. Passing the value of a, which is 2, into log(..).

Hier zie je dat de engine steeds aan scope vraagt of de expressies en variabelen er zijn.

### Nested Scope

Even samengevat dus. Een scope is een omhulsel waarin je specifieke variabelen kan aanroepen. Scopes zijn eigenlijk altijd genest. Wanneer de "engine" de gevraagde variabele niet kan vinden binnen de huidige scope gaat hij een laag omhoog. Dit blijft doorgaan tot hij bij de global scope aankomt. (je weet wel de .Window shit die hij altijd liet zien in de les.)

### Review (TL;DR)

#### altijd handig (:

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. Scope-related assignments can occur either with the = operator or by passing arguments to (assign to) function parameters.

The JavaScript Engine first compiles code before it executes, and in so doing, it splits up statements like var a = 2; into two separate steps:

- First, var a to declare it in that Scope. This is performed at the beginning, before code execution.

- Later, a = 2 to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing Scope, and if need be (that is, they don't find what they're looking for there), they work their way up the nested Scope, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in ReferenceErrors being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a ReferenceError (if in "Strict Mode" [^note-strictmode]).

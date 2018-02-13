# Aantekeningen artikelen

__Scope__ Set of rules for storing variables in some location

## Compiler theory

JS is compiled, but not well in advance.

In a traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps before it is executed, roughly called "compilation":

__1. Tokenizing/Lexing__
Breaking up a string of characters into meaningful (to the language) chunks, called tokens.

__2. Parsing__
Taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" __(Abstract Syntax Tree)__.

__3. Code-Generation__
The process of taking an AST and turning it into executable code.

JS engines don't have a lot of time to optimize because JS compilation doesn't happen in a build step ahead of time. The compilation happens mere microseconds before the code is executed.

## Understanding Scope

### The cast
Cast of characters that interact to process the program:
1. *Engine*: responsible for start-to-finish compilation and execution of our JavaScript program.
2. *Compiler*: handles all the dirty work of parsing and code-generation.
3. *Scope*: collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

### Back and forth

When you see the program var a = 2;, you most likely think of that as one statement. But that's not how our new friend Engine sees it. In fact, Engine sees two distinct statements, one which Compiler will handle during compilation, and one which Engine will handle during execution.

The first thing Compiler will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree. And next: 
1. Encountering var a, Compiler asks Scope to see if a variable a already exists for that particular scope collection. If so, Compiler ignores this declaration and moves on. Otherwise, Compiler asks Scope to declare a new variable called a for that scope collection.
2. Compiler then produces code for Engine to later execute, to handle the a = 2 assignment. The code Engine runs will first ask Scope if there is a variable called a accessible in the current scope collection. If so, Engine uses that variable. If not, Engine looks elsewhere (see nested Scope section below).

## Nested Scope

## Errors

## Review (TL;DR)

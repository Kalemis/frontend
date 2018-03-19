# Async and Performance

## Table of contents
[Asynchrony now and later](#asynchrony_now_and_later)  
[Callbacks](#callbacks)  
[Promises](#promises)  
[Generators](#generators)  
[Program performance](#program_performance)  
[Benchmarking and tuning](#benchmarking_and_tuning)  

## Asynchrony now and later

You may write your JS program in one .js file, but your program is almost certainly comprised of several chunks, only one of which is going to execute now, and the rest of which will execute later. The most common unit of chunk is the ```function```.

The simplest way of "waiting" from *now* until *later* is to use a function, commonly called a callback function.

Any time you wrap a portion of code into a ```function``` and specify that it should be executed in response to some event (timer, mouse click, Ajax response, etc.), you are creating a *later* chunk of your code, and thus introducing asynchrony to your program.


## Callbacks

## Promises

## Generators

## Program performance

## Benchmarking and tuning



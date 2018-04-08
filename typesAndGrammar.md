# Types & Grammar

## Table of contents
- [Types](#types)
- [Values](#values)
- [Natives](#natives)
- [Coercion](#coercion)
- [Grammar](#grammar)

## Types

If both the engine and developer treat value 42 (number) different than they treat value “42” (string), then those two values have different types ! 

### JS has 7 built-in types (te achterhalen door: ‘typeof’):
Null ( returns object by ‘typeof’. Because its buggy, it will never be fixed. Too much content relies on it) 

- Undefined 
- Boolean (true/false) 
- Number (42) 
- String (“ .. “) 
- Object
- Symbol (added in ES6) 

### NOTE: All of these objects are called primitives (except: object) 
Function is not an built-in type, but a subtype of an object! 

- Variables don’t have types, values have types. 
- Variables can hold any value, at any time. 
- If you use ‘typeof’ against a variable: it’s asking: 
"what's the type of the value in the variable?". 

Typeof operator: always returns a string 

**Undefined VS Undeclared**
Undefined = variable is already declared in the scope, but has no value 
Undeclared = variable not declared in the scope (reference error) 
BUT: typeof operator will always say: “Undefined”


## Values
### Arrays: 
A container of any type of value: strings, numbers, objects, another array (aka multidimensionale array)

Delete on array will remove that slot, but not update the ```.length``` property 

Array is numerically indexed, but the objects can also have a string! Which don’t count toward the ```.length``` property. 

### Numbers: 
Integer (42) and Decimal (42.2)
Numbers in JS are based on "IEEE 754" standard, often called "floating-point."
0 = optional: .42 or 42. === 42

You can use ```Number.EPSILON``` to compare two numbers for "equality" (within the rounding error tolerance)
Otherwise > 
``` javascript
0.1 + 0.2 === 0.3  // false
```

### Non-value Values: 
- Undefined > there is only one value: undefined
- Null > only one value ass well: null 
- Label is both its type and its value

### NaN = Not a Number
- You can’t directly compare it to itself
Use instead: ```isNaN(..)``` > tells us if the value is nan or not
 

## Natives
### Most commonly used natives:
* string()
* Number()
* Boolean()
* Array()
* Object()
* Function()
* RegExp()
* Date()
* Error()
* Symbol() (added in ES6) 

### Boxing wrappers: 
Primitive values don’t have properties or methods. To access ```.length``` of ```.toString()```, you need an object wrapper around the value! JS will automatically (aka wrap) the primitive value to fulfil such accesses. 

Example: ```(new String("abc"))``` is an object wrapper around the primitive ```("abc")``` value.

Want to manually box a primitive value? Use ```object(..)```, oftewel no ```new``` before. 

### Unboxing: 
Use ```valueOf()``` to get the underlying primitive value out of an object wrapper.

### Native Prototypes: 
Each of the built-in native constructors has its own ```.prototype``` object. ```array.prototype```, ```string.prototype``` etc. 

```.prototype``` is shortened to: ```# (String.prototype.XYZ = String#XYZ)```

## Coercion

### Converting a value from one type to another is: 
“Type casting”, when done explicitly  
“Coercion”, when done implicitly 

Explicit coercion = obvious  
Implicit coercion = less obvious 

Example: 
``` javascript
var b = a + "";			// implicit coercion
var c = String( a );			// explicit coercion
```

#### ToString 
When any non-string value is coerced to a string value. 
Null = “Null”, Undefined = “Undefined” 

#### ToNumber
If any non-number value is used in a way that requires it to be a number.
True = 1, False = 0, Undefined = NaN, Null = 0. 

Objects and arrays will first be converted to their primitive value and the result value will be coerced to number (if it is not already a number). 

If ```valueOf()``` is available and it returns a primitive value, that value is used for the coercion. If not, but ```toString()``` is available, it will provide the value for the coercion.

If neither operation can provide a primitive value, a TypeError is thrown.


#### ToBoolean 
Numbers are numbers and booleans are booleans. 
So 1 is not equal to true, and 0 not to false. You can coerce them, but they are not the same! 

### Falsy list: 
* undefined
* null
* false
* +0, -0, and NaN
* ""

Everything is true if it’s not on the falsy list. 


Explicitly: strings <> numbers
```B = string (a);``` 
Or: 
```B = a.toString(); ```

### || and && 
They result in the value of one of their two operands. Aka they select one of the two operand’s value’s. 

They perform a boolean test on the first operand. 
```||``` = if the test is true, result: the first operand. 
      If the test is false, result: the second operand. 

```&&``` = same as ```||```, but reversed.

What do they mean?
- ```||```: checks if the first statement **or** the second statement (after the ```||```) is true. 
- ```&&```: checks if the first statement **and** the second statement (after the ```&&```) is true.

### Symbol coercion: 
Symbol to string: Explicit = allowed. Implicit of the same is disallowed and throws an error. 

Symbol values cannot coerce to a number at all, (always an error). But can both (explicit and implicit) to a boolean. (Always true). 

Loose equals vs strict equals: 
- ```==```: allows coercion 
- ```===```, disallows coercion (checks value **and** type)

## Grammar
**Statements vs Expressions**
- Statements = sentences (var b = a) 
- Expressions = phrases (b;) 
- Operators = punctuation (and, etc, “,”, or) 

Statement contains expressions

“Expression statement” = expression and a statement by itself 

### Side effects = ? ik heb geen flauw idee, dus heb dat stukje niet echt kunnen samenvatten. Schijnt dat het amper voor komt ofzo idfk. 

### Contextual rules: 
Same syntax means different things on different places. 
Example: {} 
``` javascript
var a = {} // {} here means an object
{} = // {} here means just a regular code block (it’s valid, but not very idiomatic) 
```
### Operator Precedence 
‘How the operators are processed when there are more than one present in an expression’ like: 
```javascript
a && b || c; 		// ???
```
So what caused the behavior? Operator precedence.

Every language defines its own operator precedence list. you'd already know that ```&&``` is more precedent than ```||```.

For a complete list of operator precedence, see "Operator Precedence" on the MDN site (* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence).


### Automatic Semicolons Insertion (ASI)
When JS assumes a ```;``` in certain places, even if you didn’t put one there. 
Why? Because otherwise the program will fail, not very forgiving; 
Only effective in the presence of a new line, not in the middle of a line. 
‘Expression statements’ also accept ASI. 
Also with keywords like: break, continue, return, (es6 yield) 

### Temporal Dead Zone (TDZ) 
Using variables too early in ES6. The reference cannot be made because it hasn’t reached it’s required initialization. 

### Switch statement 
A sort-of shorthand statement for: ```if else if else``` chain. 
If the match is found, it will run the code until a break or the end of the switch blok. Just like if else. 

But: 
The matchin between the expression and each case expression is identical to ```===```. 
You can coerce it to ```==```, but you have to hack the switch statement result to something like (true). 

The default clause in the switch statement *is* optional and doesn’t have to come at the end of the switch statement.

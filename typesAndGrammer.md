## You Don't Know JS: Types & Grammar

### Built in types
JavaScript defines seven built-in types:
* null
* undefined
* boolean
* number
* string
* object
* symbol

With ```typeof``` you are able to see which type it is.  
The ```typeof``` operator always returns a string.

**Exception:**  
* Null  
The type of null is not object, but null. This is a bug in JS but too many people rely on this bug so it won't be fixed.
```javascript
typeof null === "object"; // true
```
* Function  
Function is not actually a type, but a subtype. 
```javascript
typeof function a(){ /* .. */ } === "function"; // true
```
* Array  
Also a subtype, but defined as objects
```javascript 
typeof [1,2,3] === "object"; // true
```
### Values as types
**Variables** don't have types, **values** do!

**Undefined** vs **Undeclared**  
Undefined: variable that has been declared in the accessible scope, but at the moment has no other value in it. 
Undeclared: variable that has not been formally declared in the accessible scope.
```javascript
var a;

a; // undefined
b; // ReferenceError: b is not defined
```
And more confusing 
``` javascript
var a;

typeof a; // "undefined"
typeof b; // "undefined"
```

However, the safety guard (preventing an error) on typeof when used against an undeclared variable can be helpful in certain cases.

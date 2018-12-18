# ES5 to ES6 (2015)

by Eric Mulcahy

### We will cover:
- var, let, and const
- String template literals
- Multiline strings
- `for .. of` and `for .. in`
- ES5/6 Array prototype functions
- Arrow Functions & This
- Promise 
- Map, Set vs objects and arrays. Show Jsonify of them - not good transport
- Shorthand Method Properties
- Module export/import
- Other items not covered in depth

### What era of Javascript will we cover?
This presentation will cover ES6, released in 2015. We will not cover features from ES2016, ES2017, or ES2018.
We will also not touch on Typescript. This will not be an exhaustive list of new features. Instead we will focus
on the most useful features that you ought to know.

[Javascript features and when they are available](http://kangax.github.io/compat-table/es6/)

# var, let, and const

##### What is the change?
Stop using `var`, start using `let` and `const`

##### Why?
Two reasons: first of all, when using `var`, scoping is limited to the function, but not to the block.

##### Example

```
var x = '1';
if (true) {
  var x = '2';
}
console.log(x);
```

What is logged? The variable x is redeclared within the if block.

`const` and `let` change this behavior to be more what you would expect:

```
let x = '1';
if (true) {
  let x = '2';
}
console.log(x);
```

What is logged here?

The second reason is immutability. In general it is bad practice to reuse previous variables for
new things. Doing so can lead to confusion in the code. Consider:

```
function doublePlusOne(input) {
  // todo this is not a great example
  var double = input * 2;
  double = double + 1;
  return double;
}
console.log(doublePlusOne(1));
```
In this contrived example it is no big deal. But if this method were more complicated, and at the
beginning we use the `double` variable to mean one thing, then reuse it again later to mean
something else, it becomes confusing. The variable's name of `double` is no longer accurate. It's no longer
necessary to optimize every piece of memory, and clarity of code is usually more important, so immutability
should be favored. `var` does not support immutability. So instead, we use `const`:

```
function doublePlusOne(input) {
  const double = input * 2;
  const doublePlusOne = double + 1;
  return doublePlusOne;
}
console.log(doublePlusOne(1));
```

What if we try reassigning the double variable while using `const`?

`const` is like the `final` modifier in Java. By default, most variables in Java ought to be `final`, but they are usually not,
because adding yet another keyword to declare a variable is annoying and not usually useful. In Javascript, it is now
part of the language.

Note that `const` prevents _reassigning_ the variable, but not changing the variables properties. So if you
declare an array: `const myArray = []` you can still push elements to that array. If you declare an object:
`const myObject = {prop1: 'Yes', prop2: 'Okay'}` you can still change its properties: `myObject.prop1 = 'No'`


Of course, there are times where you really want a mutable variable that you can reassign. For example, you cannot use
`const` with a basic for loop:

```
// Does not work.
function addOneThroughThree() {
  const sum = 0;
  for (const i = 1; i <= 3; i++) {
    sum += i;
  }
  return sum;
}
console.log(addOneThroughThree());
```

Instead, we need to use `let` when we want to be able to reassign the variable

```
function addOneThroughThree() {
  let sum = 0;
  for (let i = 1; i <= 3; i++) {
    sum += i;
  }
  return sum;
}
console.log(addOneThroughThree());
```

##### Conclusion

Instead of `var`, start using `const`. Unless the variable should be reassignable, then use `let`. I recommend just
using `const` everywhere until you get used to it. If you try to reassign a constant variable, linting will let you know,
and you can decide if you should use a new variable to keep immutability, or if you really do want a mutable variable.

# String Template Literals

##### What is the change?
String Template Literals are a handy way to concatenate strings with variables. This is a syntactic sugar change and is easy 
to understand.

##### Why?
It's easier to read and quicker to write. 

##### Example

```
// Without Tempalte Literals:
function buildNametag(first, last) {
  return 'Hello my name is ' + first + ' ' + last + '!';    
}
console.log(buildNametag('Eric', 'Mulcahy'));
```

is equivalent to:

```
function buildNametag(first, last) {
  return `Hello my name is ${first} ${last}!`;    
}
console.log(buildNametag('Eric', 'Mulcahy'));
```

Just use back ticks instead of single quotes, and surround your variables with dollar sign + curly braces: `${variable}`

##### Conclusion

They improve readability of code and come in handy for things like building up URLs.

# Multiline Strings

##### What is the change?
Instead of using the `+` operator to concatenate strings that span more than one line, you can now just use back ticks. 

##### Why?
No one likes concatenating strings.

##### Example
```
// Concatenating strings:
function getTermsOfService() {
    return 'I hereby agree that I will only use this Sircon product for good,\r\n'
        + 'and will not use it for evil, and I will always have all my compliance\r\n'
        + 'stuff done on time, and I will never double click submit buttons in CX\r\n';
}

console.log(getTermsOfService());
```

with multiline strings:

```
function getTermsOfService(first, last) {
    return `I hereby agree that I will only use this Sircon product for good,
        and will not use it for evil, and I will always have all my compliance
        stuff done on time, and I will never double click submit buttons in CX`;
}
console.log(getTermsOfService());
```

##### Conclusion
This is a handy way to make code a bit more readable. Keep in mind line breaks in your multiline string are treated as \r\n. 


# `for .. in` and `for .. of`

##### What is the change?
These easily confused methods are used to iterate over enumerable properties on an object or array. Let's go right to the example.

##### Examples
First, `for .. in` iterates over the 'enumerable properties' of the object or array:
```
const myNumbers = [1, 2, 5, 7, 10];
for (let x in myNumbers) {
  console.log(x);
}
// 0 1 2 3 4
```
For an array, the 'enumerable properties' are the keys in the array, which are just the integer indexes.

For an object, the 'enumerable properties' are the keys of the object that are marked as enumerable:
```
const languageRankings = {
  'Java': 3,
  'Javascript-ES5': 5,
  'Javascript-ES6': 6,
  'Typescript': 9.8  
}

for (let x in languageRankings) {
  console.log(x);
}
// Output: Java Javascript-ES5 Javascript-ES6 Typescript

for (let x in languageRankings) {
  console.log(languageRankings[x]);
}
// Output: 2 5 6 9.8
```

What about `for .. of`? This is a shorthand to iterate over iterable collections. It does not work on Objects because
objects are not iterable. 
```
const myNumbers = [1, 2, 5, 7, 10];
for (let x of myNumbers) {
  console.log(x);
}
// Output: 1 2 5 7 10
```

As you can see, this is the same as using `for .. in` where we used `console.log(languageRankings[x])` above. So instead
of iterating over the keys of an array, this iterates over the values of an array. 

##### Conclusion

These are useful methods to concisely iterate over arrays or objects. In my opinion `for .. of` feels the most natural
when used to iterate over the values of an array. `for .. in` is most useful when you want to iterate over the keys of
an object. However, both can be used for more than these two simple cases.

# Array Prototype Functions

##### What is the change?
With ES6 there are a number of new Array Prototype methods available. In many cases these replace functions previously 
available in lodash.

##### Why?
- Manipulating arrays is so common it is nice to have it built into the language, rather than only available from libraries 
like lodash or underscore.
- I think the syntax is a bit cleaner than lodash.

##### Examples

Array.prototype.find:
```
const myNumbers = [1, 2, 5, 7, 10];
const firstNumberGreaterThanFive = myNumbers.find(function(element, index, array) { return element > 5; });
console.log(firstNumberGreaterThanFive);
// 7
```

Array.prototype.keys:
```
const myNumbers = [1, 2, 5, 7, 10];
const keys = myNumbers.keys();
for (let key of keys) {
  console.log(key);
}
// 0 1 2 3 4
```

Array.prototype.values:
```
const myNumbers = [1, 2, 5, 7, 10];
const values = myNumbers.values();
for (let v of values) {
  console.log(v);
}
// 1 2 5 7 10
```

Other ES6 Prototype methods not covered here:
- `Array.from()`: Easily create a new array from an input array
- `Array.prototype.findIndex()`: Just like `Array.prototype.find` but returns the index of the element instead of the value. 


##### Other Prototype methods actually available in ES5 
- `Array.prototype.forEach`
- `Array.prototype.indexOf`
- `Array.prototype.map`
- `Array.prototype.filter`
- `Array.prototype.reduce`
- `Array.prototype.sort`

##### Should these be favored over lodash functions?
Personally, I prefer built in functions over functions from libraries. However, lodash does have some protection against nulls:
```
const myNumbers = [1, 2, 5, 7, 10];

const builtInFilter = myNumbers.filter(function(item) {return item > 5});
console.log(usingBuiltIn); // [7, 10]

const lodashFilter = _.filter(myNumbers, function(item) {return item > 5});
console.log(lodashFilter); // [7, 10]
// so far they are equivalent

const nullVariable = null;
const builtInFilter = nullVariable.filter(function(item) {return item > 5});
// Uncaught TypeError: Cannot read property 'filter' of null

const lodashFilter = _.filter(nullVariable, function(item) {return item > 5});
// empty array [] 
```

So if you want to get rid of lodash and use built in functions you do need to be careful about situations where the input 
may be null.

##### Conclusion

Many array functions previously only available from libraries like lodash are now built into the language. Just be careful
when switching because there are some subtle differences. 

# Arrow Functions
Arrow functions are shorthand function expressions. Arrow functions are easier to read and do not have their own `this` context.

### Basic Syntax

```
// Example from https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
var materials = ['Hydrogen', 'Helium', 'Lithium', 'Beryllium'];

// expected output: Array [8, 6, 7, 9]

// Function syntax:
console.log(materials.map(function(material) {return material.length}));

// Arrow function syntax:
console.log(materials.map(material => material.length));
```

- The `function` keyword is gone.
- The curly braces are gone (in this example)
- There is no `return` statement  (in this example)
- This is a nice shorthand way to represent a non-method function.  

Arrow functions are often used for in-place, not-reused functions that do not need to be methods. They are usually short,
but it is possible to have multiple statements. In that case, you do still need to use curly braces and a `return` statement:

```
console.log(materials.map(material => {
    const doubleLength = material.length * 2;
    return doubleLength;
}));
```

The parameters are optional just like with regular Javascript functions. For example all 3 of these are valid:

```
return new Promise((resolve, reject) => {
  if (true) {
    resolve(42);
  } else {
    reject('error!');
  }
});
```

```
// Parentheses are optional when there's only one parameter name:
return new Promise(resolve => {
  if (true) {
    resolve(42);
  }
});
```

```
return new Promise(resolve => resolve(42));
```

### Arrow Functions & This
This is a little confusing. 

Examples from https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions

Before arrow functions, `this` was defined based on the function. Constructors defined their own `this`. Object mehods
used the base object's `this` context. Strict mode function calls made `this` undefined. 

```
// Broken: Just prints 'NaN' to the console because this.age is undefined from the function's `this` context.

function Person() {
  // The Person() constructor defines `this` as an instance of itself.
  this.age = 0;

  setInterval(function growUp() {
    // In non-strict mode, the growUp() function defines `this` 
    // as the global object (because it's where growUp() is executed.), 
    // which is different from the `this`
    // defined by the Person() constructor. 
    this.age++;
    console.log(this.age);
  }, 1000);
}
var p = new Person();
```

In ES5, this can be fixed by assigning the constructor's `this` context to a variable:
```
// Works - prints out 1, 2, 3 etc

function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `that` variable of which
    // the value is the expected object.
    that.age++;
    console.log(that.age);
  }, 1000);
}
var p = new Person();
```

Arrow functions do not have their own `this` context. Instead they use `this` from the enclosing context: 
```
// Works without needing to keep track of the constructor's `this` context:
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the Person object
    console.log(this.age);
  }, 1000);
}
var p = new Person();
```

##### Conclusion - When to use named functions and arrow functions?

Named functions cannot be replaced entirely by arrow functions. It still makes sense to have named functions at the component
level. However, arrow functions are very useful for method callbacks. For example, functions passed to array prototype methods
can often be arrow functions. Functions passed to the .then() and .catch() for Promises can often be arrow functions 
(including http get).


# Promises

ES5 already has Promises, and most people are familiar with them. So this is not a lesson on Promises. But ES6 did introduce
a new `Promise` class to streamline the syntax and wrap functions that do not already use Promises.

```
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise 
const promise1 = new Promise((resolve, reject) => {
  setTimeout(function() {
    resolve('foo');
  }, 300);
});

promise1.then(value => console.log(value));
// expected output: "foo"
```

There is also `Promise.all`, which is pretty similar to `q.all` from AngularJS:

```
const promise1 = new Promise((resolve, reject) => {
  setTimeout(function() {
    resolve('foo');
  }, 300);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(function() {
    resolve('bar');
  }, 600);
});

Promise.all([promise1, promise2]).then(results => console.log(results));
// ["foo", "bar"]
```

# New Data Types: Map & Set

##### Sets example with ES5 syntax:
```
// Example from http://es6-features.org/#SetDataStructure
var s = {};
s["hello"] = true; s["goodbye"] = true; s["hello"] = true;
Object.keys(s).length === 2;
s["hello"] === true;
for (var key in s) // arbitrary order
    if (s.hasOwnProperty(key))
        console.log(key);
console.log(JSON.stringify(s));
```

##### Set Updated for ES6:
```
// Example from http://es6-features.org/#SetDataStructure
let s = new Set()
s.add("hello").add("goodbye").add("hello")
s.size === 2
s.has("hello") === true
for (let key of s.values()) // insertion order
    console.log(key);
console.log(JSON.stringify(s));
```

##### Map example with ES5 syntax:
```
// Example from http://es6-features.org/#MapDataStructure
var m = {};
// no equivalent in ES5
m["hello"] = 42;
// no equivalent in ES5
// no equivalent in ES5
Object.keys(m).length === 2;
for (key in m) {
    if (m.hasOwnProperty(key)) {
        var val = m[key];
        console.log(key + " = " + val);
    }
}
```

##### Map Updated for ES6:
```
// Example from http://es6-features.org/#MapDataStructure
let m = new Map()
let s = Symbol()
m.set("hello", 42)
m.set(s, 34)
m.get(s) === 34
m.size === 2
for (let [ key, val ] of m.entries())
    console.log(key + " = " + val)
```

##### Conclusion

The new data types can be handy if you want to use methods for Sets or Maps that are not available on plain Objects. However,
there are some differences especially around the JSON representation of the objects. 

# Shorthand Method Properties
As much as we love writing `function` over and over again, it is not necessary anymore for object functions:

```
// ES 5 example:
obj = {
    foo: function (a, b) {
        …
    },
    bar: function (x, y) {
        …
    }
};
```

```
// ES 6 example:
obj = {
    foo (a, b) {
        …
    },
    bar (x, y) {
        …
    }
};
```

# Module export/import
Examples from http://es6-features.org/#ValueExportImport

ES5:
```
//  lib/math.js
LibMath = {};
LibMath.sum = function (x, y) { return x + y };
LibMath.pi = 3.141593;

//  someApp.js
var math = LibMath;
console.log("2π = " + math.sum(math.pi, math.pi));

//  otherApp.js
var sum = LibMath.sum, pi = LibMath.pi;
console.log("2π = " + sum(pi, pi));
```

ES6:
```
//  lib/math.js
export function sum (x, y) { return x + y };
export var pi = 3.141593;

//  someApp.js
import * as math from "lib/math";
console.log("2π = " + math.sum(math.pi, math.pi));

//  otherApp.js
import { sum, pi } from "lib/math";
console.log("2π = " + sum(pi, pi));
```

# Other topics not covered here:
Examples from http://es6-features.org/

- Default parameters: `function f (x, y = 7, z = 42) {return x * y * z}`
- Spread operator: 
```
const params = [ "hello", true, 7 ]
const other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]
```
- Binary and Octal literals: `0b111110111 === 503    //instead of parseInt("111110111", 2) === 503`
- Property shorthand:
```
var x = 0, y = 0;
obj = { x, y };
// instead of: obj = { x: x, y: y };
```
- Classes. Lots of stuff here. See http://es6-features.org/#ClassInheritance
- Many other things - this is not an exhaustive list.
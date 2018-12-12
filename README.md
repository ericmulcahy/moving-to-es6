# ES5 to ES6 (2015)

by Eric Mulcahy

### We will cover:
- var, let, and const
- String template literals
- Multiline strings

TODO
- arrow functions - don't need the function syntax
- this & arrow functions - can't just swap all functions to arrow functions. but callbacks are safe
- array prototype functions () filter, map, reduce, foreach replaces lodash stuff
- for of vs for in
- Map, Set vs objects and arrays. Show Jsonify of them - not good transport
- Promise
- string templates ${`asdf`}

### What era of Javascript will we cover?
This presentation will cover ES6, released in 2015. We will not cover features from ES2016, ES2017, or ES2018.
We will also not touch on Typescript. This will not be an exhaustive list of new features. Instead we will focus
on the most useful features that you ought to know.

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


# Array Prototype Functions

##### What is the change?

##### Why?

##### Conclusion

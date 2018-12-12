# ES5 to ES6 (2015)

by Eric Mulcahy

### We will cover:
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

### var, let, and const

##### What is the change?
Stop using `var`, start using `let` and `const`

##### Why?
Two reasons: first of all, when using `var`, scoping is limited to the function, but not to the block.
Consider:

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


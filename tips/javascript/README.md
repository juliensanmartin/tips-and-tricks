Whenever there is a divergence between what your brain thinks is happening and what the computer does, that's where bug enter the code.

Your code is a way of communication

# Types

## Primitives types

- undefined
- string
- number
- boolean
- object
- symbol

- undeclared?
- null?
- function? (subtype of object)
- array? (subtype of object)
- bigint? (future)

## typeof

`typeof` always return a string

```js
var v;
typeof v; // "undefined"
v = '1';
typeof v; // "string"
v = true;
typeof v; // "boolean"
v = null;
typeof v; // "object" OOPS
v = function () {};
typeof v; // "function" hmmm?
v = [1, 2, 3];
typeof v; // "object" hmmm?
```

## undefined VS undeclared VS unitialized(ES6 aka TDZ)

## Special Values

- NaN: invalid number and not "Not A Number"

```js
var myAge = Number('39'); // 39
var myCatsAge = Number('n/a'); // NaN

myCatsAge === myCatsAge; // false OOPS; NaN is ther only value that is not equal to itself.

isNaN(myAge); // false
isNaN(myCatsAge); // true
isNaN("my son's age"); // true OOPS;

Number.isNaN(myCatsAge); // true
Number.isNaN("my son's age"); // false - no coercion like isNaN l.51
```

- Negative Zero:

```js
var trendRate = -0;
trendRate.toString(); // "0" OOPS
trendRate === -0; // true
trendRate === 0; // true OOPS

Object.is(trendrate, -0); // true
Object.is(trendRate, 0); // false
```

## Fundamental Objects (built-in Object, Native Functions)

Use `new`:

- Object()
- Array()
- Function()
- Date()
- RegExp()
- Error()

Don't use `new`:

- String()
- Number()
- Boolean()

```js
var yesterday = new Date('March 6, 2019');
var myGPA = String(transcript.gpa);
```

## Coercion

> type convertion

### ToPrimitive(hint)

hint what I would like to coerce to, usually int or string

valueOf() // > int
toString() // > string

### ToString -> String(...)

```js
null > "null"
undefined > "undefined"
0 > "0"
-0 > "0"
NaN > "NaN"
[] > ""
[1,2,3] > "1,2,3"
[null, undefined] > ","
[[],[],[]] > ",,,"
[,,,] > ",,,"
{} > "[object Object]"
{a:2} > "[object Object]"
{toString(){ return "X"; }} > "X" // override toString()
```

### ToNumber -> Number(...)

```js
"" > 0 // WTF not NaN?? ROOT OF ALL (Coercion) Evil
"0" > 0
"    009" > 9
"." > NaN
"Oxaf" > 175
false > 0
true > 1
null > 0
undefined > NaN // What different from null?
[""] > 0
["0"] > 0
[null] > 0
[undefined] > 0
[1,2,3] > NaN
{..} > NaN
{valueOf(){return 3}} > 3
```

### ToBoolean

Falsy OR Truthy

Falsy value:

- ""
- 0
- -0
- null
- undefined
- false

Otherwise it's truthy

### Coercion

#### String > Number

```js
function addAStudent(numStu) {
  return numStu + 1;
}

addStudent(studentsInputElem.value);
// 161 Oops!
addStudents(Number(studentsInputElem.value));
// 17
```

### Boxing

accessing function on primitive value: implicit coercion

```js
if ("huhu".length > 50) {
    ...
}
```

"huhu" is not an object and you are trying to use as if it is an object, me JS I will be helpful and make it an object for you.

### Philosophy of coercion

Implicit coercion can be seen as magic or bad but it's actually abstract.
We want to hide unnecessary details, re-focus the reader and increase clarity.

Real question to ask: is showing the reader the extra type details helpful or distracting? It depends! Sometimes yes sometimes no, it's our job as engineers to be critical and analytical thinkers.

### Exercices

1. Define an `isValidName(..)` validator that takes one parameter, `name`. The validator returns `true` if all the following match the parameter (`false` otherwise):

   - must be a string
   - must be non-empty
   - must contain non-whitespace of at least 3 characters

```js
function isValidName(name) {
  if (typeof name == 'string' && name.trim().length >= 3) {
    return true;
  }

  return false;
}
```

2. Define an `hoursAttended(..)` validator that takes two parameters, `attended` and `length`. The validator returns `true` if all the following match the two parameters (`false` otherwise):

   - either parameter may only be a string or number
   - both parameters should be treated as numbers
   - both numbers must be 0 or higher
   - both numbers must be whole numbers
   - `attended` must be less than or equal to `length`

```js
function hoursAttended(attended, length) {
  if (typeof attended == 'string' && attended.trim() != '') {
    attended = Number(attended);
  }
  if (typeof length == 'string' && length.trim() != '') {
    length = Number(length);
  }
  if (
    typeof attended == 'number' &&
    typeof length == 'number' &&
    attended >= 0 &&
    length >= 0 &&
    Number.isInteger(attended) &&
    Number.isInteger(length) &&
    attended <= length
  ) {
    return true;
  }

  return false;
}
```

## Equality

### == VS ===

https://www.ecma-international.org/ecma-262/11.0/index.html#sec-abstract-equality-comparison

- == and === are identical when the type matches
- === returns false if types are different

Difference:
== will allow coercion when types are differents
=== disable coercion when types are differents

How to choose: is coercion helpful in an equality comparaison or not?

#### ==

what happens if types are different:

- `undefined` and `null` are coercisively equals to each other
- if string, number, boolean > transform to number (ToNumber) and compare
- if not primitive > calls ToPrimitive

#### Avoid with ==

1- == with 0 or "" (or even " ")
2- == with non-primitives
3- == true or == false but Allow: ToBoolean or use ===

#### When to use ==

Knowing types is always better than not knowing them
== is about comparaisons with known types, optionally where conversions are helpful
=== if you can't/won't use known and obvious types.

## Typescript/Flow - Static Type

Benefits:
1- Catch type related mistakes
2- Communicate type intent
3- Provide IDE feedback

## Conclusion

JavaScript has a (dynamic) type system, which uses various forms of coercion for value type conversion, including equality comparaisons.

# Scope

Scope: Where to look for things

JS organizes scopes with functions and blocks(ES6)

Lexical Scope is a 2 paths processing:
1- compilation/parsing: creating buckets and marbles
2- execution

## Strict Mode

```js
'use strict';

```

In strict mode we don't create variable that were not declare in global scope when we try to assign to this variable.
It will return an `ReferenceError` if that's the case

## Function expressions VS function declaration

```js
var myThirdTeacher = function() { ... } // anonymous function expression

function teacher() { ... } // function declaration

var mySecondTeacher = function() { ... } // function expression

var myTeacher = function anotherTeacher() { // named function expression
  ...
}

console.log(teacher); // OK
console.log(myTeacher); // OK
console.log(anotherTeacher); // ReferenceError Does not exist in global scope
```

`myTeacher` is a function declaration and goes (in this case) in the global scope
`anotherTeacher` is a named function expression and goes to the function scope (not global scope)

Also `anotherTeacher` is read-only, cannot be reassign.

How to spot a function declaration: first word is `function` otherwise function expression.

## Anonymous VS Named function expressions

### Benefit of Named function expression

1- Reliable function self-reference (recursion, etc.)
2- More debuggable stack traces
3- Code more self-documented

### What about Arrow functions?

```js
var ids = people.map((person) => person.id);

var ids = people.map(function getId(person) {
  return person.id;
});
```

Arrow function are anonymous so Benefit of Named function expressions don't apply.

## Lexical scope

Scopes nested in one another. It's related to parsing/compilation time not runtime like dynamic scope.

## Function scope

How do we fix variables name collision? Create buckets aka function that will create new scopes. Hmmm maybe but the function can still have a name that later can collide.

Principle of least privilege/Principle of least exposure:
You should default everything private and only exposes what's necessary.

### IIFE Pattern (pronunce efe in En or ifi in Fr)

```js
var teacher = 'Kyle';

(function anotherTeacher() {
  // <-- function expression, function is not first word
  var teacher = 'Suzy';
  console.log(teacher); // Suzy
})();

console.log(teacher); // Kyle
```

Not polluting global scope

### Block scoping and let

```js
var teacher = 'Kyle';

{
  let teacher = 'Suzy'; // Notice use of `let`
  console.log(teacher); // Suzy
}

console.log(teacher); // Kyle
```

`let` and `const` can be declared inside of a block and turn that block into a scope.

Opposite to `var` which attach itself to the outer scope of the block.

`let` doesn't replace `var`. `let` signal a local usage, `var` signal a more general usage.

### const

```js
var teacher = 'Suzy';
teacher = 'Kyle'; // OK

const myTeacher = teacher;
myTeacher = 'Suzy'; // TypeError

const teachers = ['Kyle', 'Suzy'];
teachers[1] = 'Brian'; // Allowed!!
```

## Hoisting

Hoisting is just the by-product of compilation/parsing and then execution.

```js
var teacher = 'Kyle';
otherTeacher(); // undefined and not Kyle or ReferenceError or TypeError

function otherTeacher() {
  console.log(teacher);
  var teacher = 'Suzy';
}
```

Here `otherTeacher` is hoisted to the top so when we call it on l.2 it's working. Then on l.5 we call `teacher` which is hoisted from l.6 but not initialize yet with a value so l.2 will return `undefined`.

### let doesn't hoist the same way as var at compile time

```js
{
  teacher = 'Kyle'; // TDZ error!
  let teacher;
}
```

Difference between `let`/`const` and `var`
`var` initialize the value to `undefined` at compile time.
`let`/`const` hoist to the top but don't initialize it so cannot be touched yet.

## Closure

What is closure?
Closure is when a function "remembers" its lexical scope even when the function is executed outside that lexical scope.

```js
function ask(question) {
  setTimeout(function waitASec() {
    console.log(question);
  }, 100);
}

ask('What is closure?'); // What is closure? after 1 sec
```

Here when `waitASec` execute, `ask` has already executed and the scope attached to `ask` should have gone away but it didn't because of closure.

```js
function ask(question) {
  return function waitASec() {
    console.log(question);
  };
}

var myQuestion = ask('What is closure?');

myQuestion(); // What is closure?
```

### Closing over variables not values

```js
var teacher = 'Kyle';

var myTeacher = function () {
  console.log(teacher);
};

teacher = 'Suzy';

myTeacher(); // "Suzy" not "Kyle"
```

```js
for (var i = 1; i <= 3; i++) {
  setTimeout(function () {
    console.log(`i: ${i}`);
  }, i * 1000);
}
// i:4
// i:4
// i:4
```

How to solve this:

```js
for (var i = 1; i <= 3; i++) {
  let j = i;
  setTimeout(function () {
    console.log(`j: ${j}`);
  }, i * 1000);
}
```

OR

```js
for (let i = 1; i <= 3; i++) {
  setTimeout(function () {
    console.log(`i: ${i}`);
  }, i * 1000);
}
```

## Modules

Modules pattern needs encapsulation + group things together.
Encapsulations is the idea of hiding data and behaviours.
Only grouping things together is called a namaspace.

Modules encapsulate data and behavior (methods) together. The state(data) of a module is held by its methods via closure.

We can have module and encapsulation without closure.

### Example with IIFE - singleton because executed once

```js
var workshop = (function Module(teacher)){
   var publicAPI = {ask};
   return publicAPI;

   function ask(question){
     console.log(teacher, question);
   }
})("Kyle");

workshop.ask("It's a module, right?"); // Kyle It's a module, right?
```

### Example with functions - Factory function

```js
function WorkshopModule(teacher) {
  var publicAPI = { ask };
  return publicAPI;

  function ask(question) {
    console.log(teacher, question);
  }
}

var workshop = WorkshopModule('Kyle');

workshop.ask("It's a module, right?"); // Kyle It's a module, right?
```

### ES6 Modules

----- workshop.mjs ------

```js
var teacher = 'Kyle';

export default function ask(question) {
  console.log(teacher, question);
}
------------------------
import ask from "workshop.mjs";

ask("It's a default import, right?");
------------------------
// namespace import
import * as workshop from "workshop.mjs";

workshop.ask("It's a default import, right?");
```

Node doesn't support ES6 Modules aka uses of `require` or use extension `.mjs`

1 module per file

# Objects (Oriented)

## this

A function's this references the execution context for that call, determined entirely by how the function was called.

A this-aware function can thus have a different context each time it's called, which makes it more flexible & reusable.

```js
function ask(question){
  console.log(this.teacher, question)
}

function otherClass() {
  var myContext = {
    teacher = "Suzy"
  }
  ask.call(myContext, "Why?");
}

otherClass() // Suzy Why?
```

## 4 ways to execute a function

### Implicit binding

Here `this` point to the context that called `ask` in this case `workshop`

```js
function ask(question) {
  console.log(this.teacher, question);
}

var workshop1 = {
  teacher: 'Kyle',
  ask: ask,
};

var workshop2 = {
  teacher: 'Suzy',
  ask: ask,
};

workshop1.ask('What is implicit binding?'); // Kyle What is ...
workshop2.ask('What is implicit binding?'); // Suzy What is ...
```

### Explicit binding

Use `.call` or `.apply` on function with specific context.

```js
function ask(question) {
  console.log(this.teacher, question);
}

var workshop1 = {
  teacher: 'Kyle',
};

var workshop2 = {
  teacher: 'Suzy',
};

ask.call(workshop1, 'What is implicit binding?'); // Kyle What is ...
ask.call(workshop2, 'What is implicit binding?'); // Suzy What is ...
```

### Hard binding

Use `.bind` on `ask` will bind the wokrshop object to `ask` every time it's executed. `bind` doesn't invoke the function, it creates a new function that binds to a particular context.

```js
var workshop = {
  teacher: 'Kyle',
  ask(question) {
    console.log(this.teacher, question);
  },
};

setTimeout(workshop.ask, 10, 'Lost this?'); // undefined Lost this?
setTimeout(workshop.ask.bind(workshop), 10, 'Hard bound this?'); // Kyle Hard bound this?
```

## new keyword (constructor call)

```js
function ask(question) {
  console.log(this.teacher, question);
}

var newEmptyObject = new ask("What is 'new' doing here?");
```

The purpose of `new` is to invoke a function and use an empty object as context for `this`.

Basically same as `ask.call({}, "What is 'new' doing here?")`

### What does `new` do

1- Create a brand new empty object
2- Link\* that object to another object
3- Call fucntion with `this` pointing to the empty object
4- If function does nto return an object, assume return of `this`

## Default binding

When all the above rules don't match then `this` points to global scope.
Probably not what you want!

```js
var teacher = 'Kyle';

function ask(question) {
  console.log(this.teacher, question);
}

function askAgain(question) {
  'use strict';
  console.log(this.teacher, question);
}

ask("What's the non-strict-mode default?"); // Kyle What's the non-strict-mode default?
askAgain("What's the strict-mode default?"); // TypeError
```

## Arrow function

Arrow function don't define the `this` keyword and actually behave like a variable aka use the lexical scope.

Here `this` is lexically scope to the `ask` function which define `this` from `workshop.ask` so `this` is `workshop`

```js
var workshop = {
  teacher: 'Kyle',
  ask(question) {
    setTimeout(() => {
      console.log(this.teacher, question);
    });
  },
};

workshop.ask("Is this lexical 'this'?"); // Kyle Is this lexical 'this'?
```

### Weird cases

```js
var workshop = {
  teacher: 'Kyle',
  ask: (question) => {
    console.log(this.teacher, question);
  },
};

workshop.ask("what happened to 'this'?"); // undefined what happened to 'this'?
workshop.ask.call(workshop, "Still no 'this'?"); // undefined still no 'this'?
```

In this case the parent scope of `ask` is not `workshop` but the global scope (object are not scope).

## ES6 Class

```js
class Workshop {
  constructor(teacher) {
    this.teacher = teacher;
  }
  ask(question) {
    console.log(this.teacher, question);
  }
}

var deepJS = new Workshop('Kyle');
var reactJS = new Workshop('Suzy');

deepJS.ask("Is 'class' a class?"); // Kyle Is 'class' a class?
reactJS.ask('Is this class OK?'); // Suzy Is this class OK
```

## Prototypes

Objects are build by "Constructor calls" (via `new`) - not the same as having a `constructor`

A "constructor call" makes an object LINKED TO its own prototype.
(it's not a COPY like with the Class pattern)

### Prototype Chain

BTW this is equivalent to ES6 class

```js
function Workshop(teacher) {
  this.teacher = teacher;
}
Workshop.prototype.ask = function (question) {
  console.log(this.teacher, question);
};

var deepJS = new Workshop('Kyle');
var reactJS = new Workshop('Suzy');

deepJS.ask("Is 'class' a class?"); // Kyle Is 'class' a class?
reactJS.ask('Is this class OK?'); // Suzy Is this class OK
```

// Object exist by default
Object O -> .prototype -> P -> .constructor -> O

Workshop -> .prototype -> P1{ask} -> .constructor -> Workshop
| -> P
// new keyword
DeepJS -> {teacher} + P1{ask}
ReactJS -> {teacher} + P1{ask}

JavaScript has, on top of every program, an Object with a property `prototype` that reference the prototype object. `prototype` has a lot of utilities like `toString` etc.

```js
deepJS.constructor === Workshop;
deepJS.__proto__ === new Workshop.prototype(); // true, __proto__ is called dunder prototype. Available in the top Object
Object.getPrototypeOf(deepJS) === Workshop.prototype; // true
```

### classical VS prototypal inheritance

Classical inheritence === copy

Prototypal inheritence === linked to === Behavior delegation pattern

### OLOO Pattern

OLOO: Object linked to Other Objects !== OO: Object Oriented (class oriented)

```js
class Workshop {
  constructor(teacher) {
    this.teacher = teacher;
  }
  ask(question) {
    console.log(this.teacher, question);
  }
}

class AnotherWorkshop extends Workshop {
  speakUp(msg) {
    this.ask(msg);
  }
}

var JSRecentParts = new AnotherWorkshop('Kyle');

JSRecentParts.speakUp('Are classes getting better?'); // Kyle Are classes getting better?
```

> OLOO style

```js
var Workshop = {
  setTeacher(teacher) {
    this.teacher = teacher;
  }
  ask(question) {
    console.log(this.teacher, question);
  }
}

var AnotherWorkshop = Object.assign(
  Object.create(Workshop), // link
  {
    speakUp(msg) {
      this.ask(msg);
    }
  }
)

var JSRecentParts = Object.create(AnotherWorkshop);
JSRecentParts.setTeacher("Kyle")
JSRecentParts.speakUp('But isnt it cleaner?'); // Kyle But isnt it cleaner?
```

No dealing with `prototype` or `new` or `constructor`, just objects thanks to `Object.create()`

Delegation design pattern is a very powerful pattern.

# Other Tips

```js
throw new Error('my error string');
```

`node file.js` > execute a JS file

```js
async function foo(bar, boo){
	await fofo;
	...
}
```

```js
async () => {
	await ...
}
```

In a setup-global.js file

```js
global.test = test;
global.foo = foo;
```

`node —require ./setup-globals.js` > add function to global object of node if execute like:

`/favorite number/i` > regular expression in JS (look for "favorite number" without being case sensitive)

```html
<form>
	<label htmlFor=“username”>Username<label>
	<input id=“username” placeholder=“username” name=“username” />
</form>
```

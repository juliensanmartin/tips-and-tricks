# Types

## number, string, boolean

```js
function add(n1: number, n2: number) {
  return n1 + n2;
}

const number1 = 5;
const number2 = 4.5;

const result = add(number1, number2);
console.log(result);
```

JS uses dynamix types (resolved at runtime)
TS uses static types (set during development)

In TypeScript, you work with types like string or number all the times.
Important: It is string and number (etc.), NOT String, Number etc.
The core primitive types in TypeScript are all lowercase!

## type inference

TS will always apply the type from the value.

`const number1 = 5` === `const number1: number = 5`

## object types

```js
const person = {
    name: 'foo',
    age: 30
}
console.log(person.nickname)  // TS error from inference

// OR without infering -- avoid
const person {
    name: string,
    age: number
} = {
    name: 'foo',
    age: 30
}

```

## array types

```js
const person = {
  name: 'foo',
  age: 30,
  hobbies: ['Sports', 'Cooking'], // TS inferes > string[]
};
```

## Tuples types

array with only 2 element (key/value).

```js
const role: [number, string] = [2, 'author'];
```

## Enum types

```js
enum Role { // enum is TS keyword
    ADMIN = 'ADMIN', // if no value by default it will be 0
    READ_ONLY = 'READ_ONLY', //1
    AUTHOR = 'AUTHOR' // 2
}
```

## any type

try to avoid, because it takes away TS feature

## Union type

```js
function combine(n1: number, n2: number) {
  return n1 + n2;
}

combine(12, 24); // 36
combine('foo', 'bar') // TS Error not a number

function combine(n1: number | string, n2: number | string) {
    if (typeof input === 'number' && ...){ // need check to clarify for TS
        return n1 + n2;
    } else {
        return input1.toString() + ...
    }
}
```

## Literal type

```js
function combine(n1: number | string, n2: number | string, resultConversion: 'as-number' | 'as-text') {
    if (typeof input === 'number' && ...){ // need check to clarify for TS
        return n1 + n2;
    } else {
        return input1.toString() + ...
    }
}

// resultConversion is either the value 'as-number' or 'as-text' specifically!

combine(12, 24); // 36
combine('foo', 'bar') // 'foobar'

```

## type aliases

```js
type MyType = number | string;
type MyType2 = 'as-number' | 'as-text';
```

### object

```js
type User = { name: string, age: number };
const user1: User = { name: 'Max', age: 30 };
function greet(user: User) {
  console.log('Hi, I am ' + user.name);
}
```

## function return type

```js
function add(n1: number, n2: number): number {
  // TS infers the type anyway so not really needed here
  return n1 + n2;
}

function printResult(num: number): void {
  // TS infers `void` because no `return`
  console.log('num', num);
}
```

small confusion here with js return `undefined` and TS has a `undefined` type but they are a little different

## function as type

```js
function add(n1: number, n2: number): number {
  // TS infers the type anyway so not really needed here
  return n1 + n2;
}

const newAdd: Function = add;
const betterAdd: (a: number, b: number) => number = add; // better type
```

## unknown type

```js
let userInput: unknow;
let userName: string;

userInput: 5;
userInput: 'Max';
userName = userInput; // TS error, if the type was `any` TS will be happy
```

## never type

can be uses in return for function to be clear that the function will never return.

# TS Compiler

`tsc --init` // create a tsconfig.json

# Interfaces

```js
interface Person {
  name: string;
  age: number;
  greet(phrase: string): void;
}

let user1: Person;
```

### difference with `type`

`type` can use Union
`interface` is a contract when instantiating an object. Force to have properties of the interface. Helpful when using with `class` and `implements`.

Also interface can `extends` other interface.

```js
interface Greetable {
    name?: string;  // this is optional
    greet(phrase: string): void;
}

class Person implements Greetable, AnotherInterface, etc. {
    name: string;

    constructor(n: string) {
        this.name = n;
    }

    greet()
}

let user1 = new Person('John');
```

### interface as function

```js
type AddFn = (a: number, b: number) => number;
// OR
interface AddFn {
  (a: number, b: number): number;
}

const add: AddFn = (n1: number, n2: number) => n1 + n2;
```

# Some keywords from TS

`readonly`, `private`, `abstract`

# Advanced Types

## Intersection types

```js
type Admin = {
  name: string,
  privileges: string[],
};

type Employee = {
  name: string,
  startDate: Date,
};

type ElevatedEmployee = Employee & Admin;

// Can use `interface` and `extends` keyword
```

## Type guard

### typeof

```js
type Combinable = string | number;
type Numeric = number | boolean;

function add(n1: Combinable, n2: Combinable) {
    if (typeof n1 === 'string' || typeof n2 === 'string'){ // < type guard
        return a.toString() + ...
    } else {
        return n1+n2
    }
}
```

### Check object property exist in object

```js
type Admin = {
  name: string,
  privileges: string[],
};

type Employee = {
  name: string,
  startDate: Date,
};

type UnknownEmployee = Employee | Admin; // Union type

function printEmployee(emp: UnknownEmployee) {
  console.log(emp.name);
  // console.log(emp.privileges) // TS error
  if ('privileges' in emp) {
    // check object property exist (JS)
    console.log(emp.privileges);
  }
}
```

### `instanceof`

Use when using class (not working with interface)

```js
if (vehicle instanceof Truck){ ... }
```

### descriminated union

```js
interface Bird {
  type: 'bird'; // add a description of object
  flyingSpeed: number;
}

interface Horse {
  type: 'horse';
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  switch (animal.type) {
    case 'bird':
    //
    case 'horse':
    //
  }
}
```

### type casting

```js
// in html <p></p>
const paragraph = document.querySelector('p'); // TS infers HTMLParagraphElement

// in html <input id='foo'></input>
const userInputElement = document.getElementById('foo')!; // TS infers HTMLElement because he doesn't know on which element the if is attached to!

userInputElement.value = 'Hu';  // TS error
```

Cast v1
`const userInputElement = <HTMLInputElement> document.getElementById('foo')!;`

```
// Cast v2 JSX (React)
const userInputElement = document.getElementById('foo')! as HTMLInputElement;

// also need to use `!` after to tell TS it won't be null OR do a check
```

### Index propertie

When we don't know the actual name of the property

```js
interface ErrorContainer {
    [prop: string]: string
}

const errorBag: ErrorContianer = {
    email: 'huhu';
}
```

### Function overload

```js
type Combinable = string | number;
type Numeric = number | boolean;

function add(n1: Combinable, n2: Combinable) {
    if (typeof n1 === 'string' || typeof n2 === 'string'){ // < type guard
        return a.toString() + ...
    } else {
        return n1+n2
    }
}

const result = add('foo', 'bar'); // TS infers Combinable type not string
const result = add('foo', 'bar') as 'string';

// function overload
function add(n1: number, n2: number): number // only see by TS
function add(n1: string, n2: string): string // only see by TS

const result = add('foo', 'bar'); // TS knows it's a string
```

### Optional chaining operator `?`

```js
user?.job?.title;
```

### Nullish Coalescing `??`

```js
const userInput = '';

const storedDatat = userInput ?? 'DEFAULT'; // check if null or undefined BUT not falsy (empty string, 0)
```

# Generics - Flexibitity + Safety

```js
const names: Array<string> = []; // string[]
const names = ['foo', 'bar']; // infers string[]

const promise: Promise<string> = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Done!'); // here is the string
  }, 2000);
});
// useful here
promise.then((data) => {
  data.split(); // TS knows it's a string from ^
});
```

## Generic Function

```js
function merge(a: object, b: object) {
  return Object.assign(a, b);
}

const mergeObj = merge({ foo: 'bar' }, { huu: 'haha' }); // {foo: 'bar', huu: 'haha'}
mergeObj.foo; // Hmmm! TS doesn't know `foo`

// FIX That:
function merge<T, U>(a: T, b: U) {
  // Now TS infers the return is T & U
  return Object.assign(a, b);
}

const mergeObj = merge({ foo: 'bar' }, { huu: 'haha' }); // {foo: 'bar', huu: 'haha'}
mergeObj.foo; // TS know `foo` now
```

## Generics with constraint - `extends` on generic

```js
function merge<T extends object, U extends object>(a: T, b: U){ // Now TS infers the return is T & U
    return Object.assign(a, b);
}

const mergeObj = merge({foo: 'bar'}, 30) // TS error because 30 is not an obejct from constraint
```

### `keyof` constraint

```js
function extract<T extends object, U extends string>(obj: T, key: U){
    return obj[key] // TS cannot check if key exist in obj
}

extract({ name: 'Max'}, 'name');

//FIX
function extract<T extends object, U extends keyof T>(obj: T, key: U){
    return obj[key] // error gone
}

extract({ name: 'Max'}, 'name');
```

## Generic Object

```js
class DataStorage<T> {
    private data: T[] = [];

    addItem(item: T){
        this.data.push(item);
    }

    removeItem(item: T){
        this.data.splice(this.data.indexOf(item), 1)
    }

    getItems() {
        return [...this.data];
    }
}

const textStorage = new DataStorage<string>();
textStorgae.addItem(10); // TS error
```

## Generic Utility Types

### Partial

```js
interface Foo {
    baz: string;
}

functiob createFoo(title: string): Foo {
    let bar: Partial<Foo> = {} // Partial turned the interface like the properties are optional
    bar.baz = 'hu';
    return bar as Foo; // Need to cast back otherwise TS error
}
```

### ReadOnly

# Decorators

use `experimentalDecorators` in tsconfig.json

# Modules

use `"modules": "es2015"` in tsconfig.json
`<script type="module" src="dist/app.js">`

To use a third party we need to use a translated version of the library using the npm package `@types` (ie @types/lodash)

In case the @types is not available we can manually do it with `declare`

```js
declare var foo: string;
```

# React

`React.FC` = type for Function Component

`const TodoList: React.FC<TodoListProps> = props => {...}`

ref type: `useRef<whatever type>(null)`

callback type: `onAddTodo: (todoText: string) => void`

useState: `useState<Todo[]>([])`

# DOM

`HTMLElement`
`document.getElementById`
`document.querySelector('#foo')`
`document.addEventListener('click', handler)` // don't forget bind if inside a class with this

`event.preventDefault()`

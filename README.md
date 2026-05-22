# js-es6


# JavaScript & ES6 Important Interview Questions

## Table of Contents
1. [Basic JavaScript Concepts](#basic-javascript-concepts)
2. [ES6 Features](#es6-features)
3. [Asynchronous JavaScript](#asynchronous-javascript)
4. [Object-Oriented Programming](#object-oriented-programming)
5. [Functional Programming](#functional-programming)
6. [DOM & Browser APIs](#dom--browser-apis)
7. [Performance & Optimization](#performance--optimization)

---

## Basic JavaScript Concepts

### 1. What is hoisting?
**Answer:** Hoisting is JavaScript's behavior of moving declarations to the top of their scope before code execution.
- `var` declarations are hoisted and initialized with `undefined`
- `let` and `const` are hoisted but not initialized (Temporal Dead Zone)
- Function declarations are fully hoisted

```javascript
// var example
console.log(x); // undefined
var x = 5;

// let example
console.log(y); // ReferenceError
let y = 10;
```

### 2. Explain the difference between var, let, and const
| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Re-declaration | Yes | No | No |
| Re-assignment | Yes | Yes | No |

### 3. What is closure?
**Answer:** A closure is a function that has access to variables from its outer scope even after the outer function has returned.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

### 4. What is the difference between == and ===?
- `==` compares values with type coercion
- `===` compares values without type coercion (strict equality)

```javascript
5 == '5'   // true (type coercion)
5 === '5'  // false (strict)
```

### 5. Explain the event loop
**Answer:** The event loop is JavaScript's mechanism for handling asynchronous operations.
- **Call Stack:** Executes synchronous code
- **Callback Queue:** Holds callbacks from async operations
- **Event Loop:** Checks if stack is empty, then moves callbacks to stack

Order of execution:
1. Synchronous code (call stack)
2. Microtasks (Promises, queueMicrotask)
3. Macrotasks (setTimeout, setInterval)

### 6. What is the difference between null and undefined?
- `undefined` - variable declared but not assigned
- `null` - intentional absence of value (must be assigned explicitly)

```javascript
let x; // undefined
let y = null; // null
```

### 7. Explain 'this' keyword
**Answer:** `this` refers to the object that the function is called on.
- Global scope: `this` = window (browser) or global (Node.js)
- Object method: `this` = the object
- Constructor: `this` = new instance
- Arrow functions: `this` is inherited from parent scope

```javascript
const obj = {
  name: 'John',
  greet: function() {
    console.log(this.name); // 'John'
  },
  greetArrow: () => {
    console.log(this.name); // undefined (inherits from outer scope)
  }
};
```

### 8. What is type coercion?
**Answer:** Automatic conversion of one data type to another.

```javascript
'5' + 3     // '53' (string concatenation)
'5' - 3     // 2 (numeric operation)
true + 1    // 2
null + 1    // 1
undefined + 1 // NaN
```

---

## ES6 Features

### 9. What are arrow functions?
Arrow functions provide a concise syntax and inherit `this` from the parent scope.

```javascript
// Traditional
const add = function(a, b) {
  return a + b;
};

// Arrow
const add = (a, b) => a + b;
const square = x => x * x;
```

**Key differences:**
- No `arguments` object
- Cannot be used as constructors
- No `prototype` property

### 10. What are template literals?
Template literals use backticks and allow string interpolation.

```javascript
const name = 'John';
const age = 30;

// Traditional
const message = 'Hello ' + name + ', you are ' + age + ' years old';

// Template literal
const message = `Hello ${name}, you are ${age} years old`;

// Multi-line
const html = `
  <div>
    <p>${name}</p>
  </div>
`;
```

### 11. Explain destructuring
Destructuring allows extracting values from objects and arrays into distinct variables.

```javascript
// Object destructuring
const person = { name: 'John', age: 30, city: 'NYC' };
const { name, age } = person;

// Array destructuring
const colors = ['red', 'green', 'blue'];
const [first, second] = colors;

// With defaults
const { name = 'Unknown', country = 'USA' } = person;

// Rest operator
const { name, ...rest } = person;
```

### 12. What is the spread operator?
The spread operator (`...`) spreads iterable elements.

```javascript
// Array
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];

// Object
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };

// Function arguments
const numbers = [1, 2, 3];
Math.max(...numbers); // 3
```

### 13. What are default parameters?
Default parameters provide default values if not supplied.

```javascript
const greet = (name = 'Guest', age = 18) => {
  console.log(`Hello ${name}, age ${age}`);
};

greet(); // Hello Guest, age 18
greet('John', 25); // Hello John, age 25
```

### 14. What are classes in ES6?
Classes provide syntactic sugar for constructor functions and prototypal inheritance.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound`);
  }

  static info() {
    console.log('This is an animal');
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks`);
  }
}

const dog = new Dog('Rex', 'Labrador');
dog.speak(); // Rex barks
Animal.info(); // This is an animal
```

### 15. What are Promises?
Promises represent eventual completion/failure of async operation.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Success!');
  }, 1000);
});

promise
  .then(result => console.log(result))
  .catch(error => console.log(error))
  .finally(() => console.log('Done'));
```

**Promise methods:**
- `Promise.all()` - all must resolve
- `Promise.race()` - first to settle
- `Promise.allSettled()` - all settle (success/failure)
- `Promise.any()` - first to resolve

### 16. What is async/await?
Async/await provides cleaner syntax for handling Promises.

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.log('Error:', error);
  }
}

fetchData().then(data => console.log(data));
```

**Key points:**
- `async` function always returns a Promise
- `await` pauses execution until Promise settles
- Must use try/catch for error handling

### 17. What are modules (import/export)?
Modules allow code organization and reusability.

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export default { add, subtract };

// main.js
import math, { add, subtract } from './math.js';
import * as operations from './math.js';

add(5, 3); // 8
operations.subtract(5, 3); // 2
```

### 18. What are rest parameters?
Rest parameters capture remaining arguments into an array.

```javascript
function sum(first, second, ...rest) {
  console.log(first);  // 1
  console.log(second); // 2
  console.log(rest);   // [3, 4, 5]
  return first + second + rest.reduce((a, b) => a + b, 0);
}

sum(1, 2, 3, 4, 5); // 15
```

### 19. What is for...of and for...in?
- `for...in` - iterates over object keys (enumerable properties)
- `for...of` - iterates over iterable values (arrays, strings)

```javascript
const obj = { a: 1, b: 2 };
const arr = [10, 20, 30];

for (let key in obj) {
  console.log(key); // 'a', 'b'
}

for (let value of arr) {
  console.log(value); // 10, 20, 30
}
```

### 20. What are Map and Set?
**Map:** Stores key-value pairs with any type of key
**Set:** Stores unique values

```javascript
// Map
const map = new Map();
map.set('name', 'John');
map.set(1, 'one');
map.get('name'); // 'John'
map.has(1); // true
map.delete(1);
map.size; // 1

// Set
const set = new Set([1, 2, 2, 3, 3]);
console.log(set); // Set { 1, 2, 3 }
set.add(4);
set.has(2); // true
set.delete(2);
set.size; // 3
```

---

## Asynchronous JavaScript

### 21. What is callback hell (pyramid of doom)?
**Answer:** Multiple nested callbacks making code hard to read and maintain.

```javascript
// Callback Hell
getData(function(a) {
  getMoreData(a, function(b) {
    getMoreData(b, function(c) {
      getMoreData(c, function(d) {
        // Too many nested levels
      });
    });
  });
});

// Solution: Promises or Async/Await
const result = await getData();
const moreData = await getMoreData(result);
```

### 22. Explain setTimeout and setInterval
- `setTimeout(callback, delay)` - executes once after delay
- `setInterval(callback, delay)` - executes repeatedly at intervals

```javascript
setTimeout(() => {
  console.log('After 1 second');
}, 1000);

const intervalId = setInterval(() => {
  console.log('Every second');
}, 1000);

// Stop interval
clearInterval(intervalId);
```

### 23. What is the difference between microtask and macrotask queues?
| Microtasks | Macrotasks |
|-----------|-----------|
| Promises | setTimeout |
| queueMicrotask | setInterval |
| MutationObserver | setImmediate |
| | requestAnimationFrame |

Microtasks execute before macrotasks in the event loop.

```javascript
console.log('Start');

setTimeout(() => console.log('setTimeout'), 0);
Promise.resolve().then(() => console.log('Promise'));
console.log('End');

// Output:
// Start
// End
// Promise
// setTimeout
```

### 24. What is fetch API?
Fetch provides a modern way to make HTTP requests.

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) throw new Error('Network response failed');
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.log('Error:', error));

// With async/await
async function getData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.log('Error:', error);
  }
}
```

### 25. What is callback function?
A callback is a function passed as argument to another function.

```javascript
function processUserInput(callback) {
  const name = prompt('Enter your name:');
  callback(name);
}

processUserInput(function(name) {
  console.log(`Hello ${name}`);
});

// With arrow function
processUserInput(name => console.log(`Hello ${name}`));
```

---

## Object-Oriented Programming

### 26. What is prototype and prototypal inheritance?
**Answer:** Every object has a `prototype` property that points to parent object. Prototypal inheritance allows objects to inherit properties from other objects.

```javascript
const parent = {
  greet() {
    return `Hello, ${this.name}`;
  }
};

const child = Object.create(parent);
child.name = 'John';
console.log(child.greet()); // 'Hello, John'

// Checking prototype chain
console.log(child.__proto__ === parent); // true
console.log(Object.getPrototypeOf(child) === parent); // true
```

### 27. What is constructor function?
Constructor function creates objects with shared properties and methods.

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const person1 = new Person('John', 30);
console.log(person1.greet()); // 'Hello, I'm John'
```

### 28. What is Object.create()?
`Object.create()` creates a new object with specified prototype.

```javascript
const proto = {
  greet() { return 'Hello'; }
};

const obj = Object.create(proto);
console.log(obj.greet()); // 'Hello'

// With property descriptors
const obj2 = Object.create(proto, {
  name: { value: 'John', writable: true }
});
```

### 29. What is Object.assign()?
`Object.assign()` copies properties from source to target object.

```javascript
const target = { a: 1 };
const source = { b: 2, c: 3 };

Object.assign(target, source);
console.log(target); // { a: 1, b: 2, c: 3 }

// Shallow copy
const copy = Object.assign({}, original);
```

### 30. What is instanceof operator?
`instanceof` checks if object is instance of class/constructor.

```javascript
class Animal {}
class Dog extends Animal {}

const dog = new Dog();
console.log(dog instanceof Dog);    // true
console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true
```

---

## Functional Programming

### 31. What is pure function?
Pure function returns same output for same input and has no side effects.

```javascript
// Pure
const add = (a, b) => a + b;

// Impure (side effect: console.log)
const addAndLog = (a, b) => {
  console.log(`Adding ${a} and ${b}`);
  return a + b;
};

// Impure (depends on external state)
let count = 0;
const increment = () => ++count;
```

### 32. What is higher-order function?
Function that takes function as argument or returns function.

```javascript
// Takes function as argument
const map = (arr, fn) => arr.map(fn);
map([1, 2, 3], x => x * 2); // [2, 4, 6]

// Returns function
const multiply = (a) => (b) => a * b;
const double = multiply(2);
console.log(double(5)); // 10
```

### 33. What is currying?
Currying converts function taking multiple arguments into sequence of functions.

```javascript
// Regular function
const add = (a, b, c) => a + b + c;

// Curried
const curriedAdd = (a) => (b) => (c) => a + b + c;
const addTwo = curriedAdd(2);
const addTwoAndThree = addTwo(3);
console.log(addTwoAndThree(4)); // 9

// Using reduce
const curry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return (...nextArgs) => curried(...args, ...nextArgs);
    }
  };
};
```

### 34. What is function composition?
Combining functions to create new function.

```javascript
const compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

const add = (x) => x + 1;
const multiply = (x) => x * 2;
const square = (x) => x * x;

const composedFn = compose(square, multiply, add);
console.log(composedFn(5)); // ((5 + 1) * 2) ^ 2 = 144

const pipedFn = pipe(add, multiply, square);
console.log(pipedFn(5)); // ((5 + 1) * 2) ^ 2 = 144
```

### 35. What is memoization?
Caching function results to avoid redundant calculations.

```javascript
const memoize = (fn) => {
  const cache = {};
  return (...args) => {
    const key = JSON.stringify(args);
    if (key in cache) {
      return cache[key];
    }
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
};

const fibonacci = memoize((n) => {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});
```

### 36. What are array methods (map, filter, reduce)?
- `map()` - transforms each element
- `filter()` - filters elements based on condition
- `reduce()` - reduces to single value

```javascript
const arr = [1, 2, 3, 4, 5];

const doubled = arr.map(x => x * 2); // [2, 4, 6, 8, 10]
const evens = arr.filter(x => x % 2 === 0); // [2, 4]
const sum = arr.reduce((acc, x) => acc + x, 0); // 15

// Chaining
const result = arr
  .filter(x => x > 2)
  .map(x => x * 2)
  .reduce((acc, x) => acc + x, 0); // 36
```

### 37. What is forEach vs map?
- `forEach` - executes function, returns undefined
- `map` - returns new array with transformed elements

```javascript
const arr = [1, 2, 3];

const result1 = arr.forEach(x => x * 2); // undefined
const result2 = arr.map(x => x * 2); // [2, 4, 6]

// forEach modifies external state
let sum = 0;
arr.forEach(x => sum += x); // 6

// map is pure
const sums = arr.map((x, i) => arr.slice(0, i + 1).reduce((a, b) => a + b));
```

---

## DOM & Browser APIs

### 38. What is event delegation?
Using single event listener on parent to handle events from children.

```javascript
// Without delegation
const buttons = document.querySelectorAll('button');
buttons.forEach(btn => {
  btn.addEventListener('click', () => console.log('clicked'));
});

// With delegation (more efficient)
document.addEventListener('click', (e) => {
  if (e.target.tagName === 'BUTTON') {
    console.log('clicked');
  }
});
```

### 39. What is the difference between stopPropagation and preventDefault?
- `stopPropagation()` - prevents event bubbling
- `preventDefault()` - prevents default browser action

```javascript
// stopPropagation
element.addEventListener('click', (e) => {
  e.stopPropagation(); // Prevents bubbling to parent
  console.log('Inner clicked');
});

// preventDefault
form.addEventListener('submit', (e) => {
  e.preventDefault(); // Prevents form submission
  console.log('Form not submitted');
});
```

### 40. What is localStorage and sessionStorage?
Both store key-value data on client side.

```javascript
// localStorage - persists until manually deleted
localStorage.setItem('key', 'value');
console.log(localStorage.getItem('key')); // 'value'
localStorage.removeItem('key');
localStorage.clear();

// sessionStorage - cleared when tab closes
sessionStorage.setItem('key', 'value');
console.log(sessionStorage.getItem('key'));
sessionStorage.removeItem('key');
```

---

## Performance & Optimization

### 41. What is debouncing?
Delays function execution until specified time passes without calls.

```javascript
const debounce = (fn, delay) => {
  let timeoutId;
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
};

const handleSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 500);

// Only executes after 500ms of inactivity
input.addEventListener('input', (e) => handleSearch(e.target.value));
```

### 42. What is throttling?
Limits function execution to specified time intervals.

```javascript
const throttle = (fn, interval) => {
  let lastCall = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastCall >= interval) {
      fn(...args);
      lastCall = now;
    }
  };
};

const handleScroll = throttle(() => {
  console.log('Scrolling');
}, 1000);

window.addEventListener('scroll', handleScroll);
```

### 43. What is lazy loading?
Loading resources only when needed.

```javascript
// Image lazy loading
<img src="placeholder.jpg" data-src="actual.jpg" loading="lazy" />

// Intersection Observer
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.src = entry.target.dataset.src;
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => observer.observe(img));
```

### 44. What is the difference between shallow and deep copy?
- **Shallow copy** - copies first level, nested objects still referenced
- **Deep copy** - copies all levels recursively

```javascript
const original = { a: 1, b: { c: 2 } };

// Shallow copy
const shallow = { ...original };
shallow.b.c = 3; // Affects original
console.log(original.b.c); // 3

// Deep copy
const deep = JSON.parse(JSON.stringify(original));
deep.b.c = 3; // Doesn't affect original
console.log(original.b.c); // 2

// Deep copy function
const deepCopy = (obj) => {
  if (obj === null || typeof obj !== 'object') return obj;
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map(item => deepCopy(item));
  if (obj instanceof Object) {
    const copied = {};
    for (let key in obj) {
      copied[key] = deepCopy(obj[key]);
    }
    return copied;
  }
};
```

### 45. What is SOLID principles?
**S** - Single Responsibility, **O** - Open/Closed, **L** - Liskov Substitution, **I** - Interface Segregation, **D** - Dependency Inversion

```javascript
// Single Responsibility
class User {
  constructor(name) { this.name = name; }
}
class UserRepository {
  save(user) { /* save logic */ }
}

// Open/Closed (open for extension, closed for modification)
class Shape { }
class Circle extends Shape { getArea() { } }
class Rectangle extends Shape { getArea() { } }

// Don't modify original, extend instead
```

---

## Advanced Topics

### 46. What is WeakMap and WeakSet?
Similar to Map/Set but with garbage-collectible weak references.

```javascript
const weakMap = new WeakMap();
const obj = { name: 'John' };
weakMap.set(obj, 'metadata');
console.log(weakMap.get(obj)); // 'metadata'

// When obj is garbage collected, weakMap entry is removed
```

### 47. What is proxy?
Proxy allows intercepting and customizing operations on objects.

```javascript
const target = { name: 'John', age: 30 };
const handler = {
  get(target, property) {
    console.log(`Getting ${property}`);
    return target[property];
  },
  set(target, property, value) {
    console.log(`Setting ${property} to ${value}`);
    target[property] = value;
  }
};

const proxy = new Proxy(target, handler);
proxy.name; // Logs: Getting name
proxy.age = 31; // Logs: Setting age to 31
```

### 48. What is Symbol?
Symbol creates unique identifiers.

```javascript
const id = Symbol('id');
const obj = {
  [id]: 'secret',
  name: 'John'
};

console.log(obj[id]); // 'secret'
console.log(Object.keys(obj)); // ['name'] (symbols not included)

// Well-known symbols
const iterable = {
  [Symbol.iterator]() {
    // Iterator implementation
  }
};
```

### 49. What is Generator function?
Generator function yields multiple values over time.

```javascript
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }

// With for...of
for (let value of generator()) {
  console.log(value); // 1, 2, 3
}
```

### 50. What is regular expression (RegExp)?
Pattern matching for string operations.

```javascript
const regex = /pattern/flags;

// Test if matches
/^[a-z]+$/.test('hello'); // true

// Find matches
'hello world'.match(/\w+/g); // ['hello', 'world']

// Replace
'hello world'.replace(/o/g, '0'); // 'hell0 w0rld'

// Split
'a,b,c'.split(/,/); // ['a', 'b', 'c']

// Flags: g (global), i (ignore case), m (multiline), s (dotAll)
```

---

## Common Tricky Questions

### 51. What will be the output?
```javascript
console.log(0.1 + 0.2 === 0.3); // false (floating point precision)

// Solution
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON; // true
```

### 52. What about this?
```javascript
const person = {
  name: 'John',
  getName: function() {
    return this.name;
  }
};

const getName = person.getName;
console.log(getName()); // undefined (this is window/global)

// Solution: bind, call, apply
console.log(getName.bind(person)()); // 'John'
```

### 53. Variable hoisting example:
```javascript
function test() {
  console.log(typeof x); // 'undefined' (hoisted)
  var x = 5;
  console.log(x); // 5
}

// Equivalent to:
function test() {
  var x; // hoisted, initialized as undefined
  console.log(typeof x); // 'undefined'
  x = 5;
  console.log(x); // 5
}
```

### 54. What about this object reference:
```javascript
let obj = { count: 0 };
let ref = obj;
ref.count++;
console.log(obj.count); // 1 (same reference)
```

### 55. Promise timing:
```javascript
console.log('1');
Promise.resolve().then(() => console.log('2'));
setTimeout(() => console.log('3'), 0);
console.log('4');

// Output: 1, 4, 2, 3 (microtask before macrotask)
```

---

## Tips for Interview

1. **Code quality** - Write clean, readable code
2. **Explain your thinking** - Communicate while coding
3. **Ask clarifying questions** - Don't assume
4. **Optimize if possible** - Consider time/space complexity
5. **Test edge cases** - Think about null, undefined, empty inputs
6. **Know trade-offs** - Discuss pros and cons of approaches
7. **Refresh concepts** - Focus on understanding, not memorization
8. **Practice problems** - LeetCode, HackerRank, CodeWars
9. **Read documentation** - MDN is your best friend
10. **Stay calm** - Interviews are conversations, not interrogations

---

## Resources

- **MDN Web Docs**: https://developer.mozilla.org/en-US/
- **JavaScript.info**: https://javascript.info/
- **You Don't Know JS**: Free book series
- **Eloquent JavaScript**: Free online book
- **LeetCode**: https://leetcode.com/
- **HackerRank**: https://www.hackerrank.com/






# React Interview Questions & Answers

## Table of Contents
1. [React Basics](#react-basics)
2. [Components & JSX](#components--jsx)
3. [Props & State](#props--state)
4. [Lifecycle Methods](#lifecycle-methods)
5. [Hooks](#hooks)
6. [Event Handling](#event-handling)
7. [Forms & Validation](#forms--validation)
8. [Performance Optimization](#performance-optimization)
9. [Routing](#routing)
10. [State Management](#state-management)
11. [Advanced Topics](#advanced-topics)

---

## React Basics

### 1. What is React?
**Answer:** React is a JavaScript library for building user interfaces using reusable components. It uses a virtual DOM to efficiently update the actual DOM.

**Key features:**
- Component-based architecture
- Virtual DOM for performance
- Unidirectional data flow
- Declarative programming

```jsx
import React from 'react';

function App() {
  return <h1>Hello, React!</h1>;
}

export default App;
```

### 2. What is the Virtual DOM?
**Answer:** Virtual DOM is a lightweight copy of the real DOM kept in memory. React uses it to optimize performance by:
1. Comparing old and new Virtual DOM (diffing)
2. Updating only changed elements in real DOM (reconciliation)
3. Batching updates for efficiency

```javascript
// React compares virtual DOMs
// Old: <div>Hello</div>
// New: <div>Hello World</div>
// Only text content is updated, not entire element
```

### 3. What is JSX?
**Answer:** JSX is syntax extension for JavaScript that looks like HTML. It gets compiled to JavaScript function calls.

```jsx
// JSX
const element = <h1>Hello {name}</h1>;

// Compiled to:
const element = React.createElement('h1', null, `Hello ${name}`);

// Can use expressions in JSX
const element = (
  <div>
    <h1>{name}</h1>
    <p>{1 + 1}</p>
    <button onClick={handleClick}>Click me</button>
  </div>
);
```

**Rules:**
- Return single root element
- Use `className` instead of `class`
- Use `htmlFor` instead of `for`
- Self-closing tags must have `/>`
- Attributes are camelCase

### 4. What is the difference between React functional and class components?

| Feature | Functional | Class |
|---------|-----------|-------|
| Syntax | Function | Class extending React.Component |
| State | Hooks (useState) | this.state |
| Lifecycle | Hooks (useEffect) | Lifecycle methods |
| This binding | No | Yes |
| Performance | Better | Slightly heavier |
| Readability | Simpler | More verbose |

```jsx
// Functional Component
function Welcome(props) {
  return <h1>Hello {props.name}</h1>;
}

// Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello {this.props.name}</h1>;
  }
}
```

### 5. What is the difference between controlled and uncontrolled components?

**Controlled Component:** React state controls the input value
```jsx
function Form() {
  const [name, setName] = useState('');

  return (
    <input 
      value={name} 
      onChange={(e) => setName(e.target.value)}
    />
  );
}
```

**Uncontrolled Component:** DOM controls the input value
```jsx
function Form() {
  const inputRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(inputRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} />
      <button>Submit</button>
    </form>
  );
}
```

### 6. What is React Fragment?
**Answer:** Fragment allows grouping multiple elements without adding extra DOM nodes.

```jsx
// Using Fragment
<>
  <Header />
  <Main />
  <Footer />
</>

// Equivalent to:
<React.Fragment>
  <Header />
  <Main />
  <Footer />
</React.Fragment>

// With key (for lists)
{items.map((item) => (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </React.Fragment>
))}
```

### 7. What is strict mode in React?
**Answer:** StrictMode highlights potential problems during development.

```jsx
import React from 'react';

function App() {
  return (
    <React.StrictMode>
      <Header />
      <Content />
      <Footer />
    </React.StrictMode>
  );
}
```

**It identifies:**
- Unsafe lifecycles
- Legacy string ref API
- Unexpected side effects
- Unsafe context API

### 8. What is React.memo?
**Answer:** React.memo memoizes a component to prevent unnecessary re-renders.

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  return <div>{props.name}</div>;
});

// Only re-renders if props change

// Custom comparison
const MyComponent = React.memo(
  (props) => <div>{props.name}</div>,
  (prevProps, nextProps) => {
    return prevProps.name === nextProps.name;
  }
);
```

---

## Components & JSX

### 9. What are props?
**Answer:** Props are arguments passed into React components. They are read-only.

```jsx
// Parent Component
function App() {
  return <Welcome name="John" age={30} />;
}

// Child Component
function Welcome(props) {
  return <h1>Hello {props.name}, age {props.age}</h1>;
}

// Destructuring
function Welcome({ name, age }) {
  return <h1>Hello {name}, age {age}</h1>;
}

// Default props
function Welcome({ name = 'Guest', age = 18 }) {
  return <h1>Hello {name}, age {age}</h1>;
}

// PropTypes validation
import PropTypes from 'prop-types';

Welcome.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
};
```

### 10. What is the difference between presentational and container components?

**Presentational Components:**
- Focus on how things look
- Receive data via props
- No state (usually)
- Reusable
- Easy to test

```jsx
const Button = ({ text, onClick }) => (
  <button onClick={onClick}>{text}</button>
);
```

**Container Components:**
- Focus on how things work
- Manage state
- Fetch data
- Pass data to presentational components

```jsx
function ButtonContainer() {
  const [count, setCount] = useState(0);

  return <Button text={`Count: ${count}`} onClick={() => setCount(count + 1)} />;
}
```

### 11. What are Higher Order Components (HOC)?
**Answer:** HOC is a pattern for reusing component logic by wrapping a component.

```jsx
// HOC definition
const withTheme = (Component) => {
  return (props) => {
    const [theme, setTheme] = useState('light');

    return (
      <div className={theme}>
        <Component {...props} theme={theme} setTheme={setTheme} />
      </div>
    );
  };
};

// Using HOC
const MyComponent = ({ theme, setTheme }) => (
  <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
    Toggle Theme
  </button>
);

const MyComponentWithTheme = withTheme(MyComponent);
```

**Use cases:**
- Code reuse
- Props manipulation
- State abstraction
- Conditional rendering

### 12. What is the Render Props pattern?
**Answer:** Render Props pass a function as prop that returns JSX.

```jsx
// Component with render prop
const MouseTracker = ({ render }) => {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (e) => {
    setPosition({ x: e.clientX, y: e.clientY });
  };

  return (
    <div onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
};

// Using render prop
function App() {
  return (
    <MouseTracker 
      render={({ x, y }) => (
        <h1>Mouse position: ({x}, {y})</h1>
      )}
    />
  );
}

// Alternative: children as render prop
<MouseTracker>
  {({ x, y }) => <h1>Mouse position: ({x}, {y})</h1>}
</MouseTracker>
```

### 13. What is composition?
**Answer:** Building complex components from simpler components.

```jsx
// Simple components
const Header = () => <header>Header</header>;
const Main = () => <main>Main Content</main>;
const Footer = () => <footer>Footer</footer>;

// Composed component
function Page() {
  return (
    <>
      <Header />
      <Main />
      <Footer />
    </>
  );
}

// Composition with children
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      {children}
    </div>
  );
}

<Card title="My Card">
  <p>This is the content</p>
</Card>
```

### 14. What is the key prop?
**Answer:** Key helps React identify which items have changed, been added, or removed.

```jsx
// Good: Using unique ID
const items = [
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' }
];

{items.map(item => <Item key={item.id} item={item} />)}

// Bad: Using index (if list can be reordered)
{items.map((item, index) => <Item key={index} item={item} />)}

// Benefits:
// - Maintains component state correctly
// - Better performance
// - Prevents bugs in forms/inputs
```

---

## Props & State

### 15. What is state?
**Answer:** State is data that changes over time and triggers re-renders.

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**State vs Props:**
| State | Props |
|-------|-------|
| Mutable | Immutable |
| Managed by component | Passed from parent |
| Causes re-render | Causes re-render |
| Local to component | Passed to children |

### 16. What is lifting state up?
**Answer:** Moving state to common parent when multiple components need same state.

```jsx
// Before: State in each component
function Celsius() {
  const [celsius, setCelsius] = useState('');
  return <input value={celsius} onChange={(e) => setCelsius(e.target.value)} />;
}

// After: State lifted to parent
function TemperatureConverter() {
  const [celsius, setCelsius] = useState('');

  const fahrenheit = (celsius * 9/5) + 32;

  return (
    <div>
      <Celsius value={celsius} onChange={setCelsius} />
      <Fahrenheit value={fahrenheit} />
    </div>
  );
}

function Celsius({ value, onChange }) {
  return <input value={value} onChange={(e) => onChange(e.target.value)} />;
}

function Fahrenheit({ value }) {
  return <input value={value} readOnly />;
}
```

### 17. How do you pass data from child to parent?
**Answer:** Using callback functions passed as props.

```jsx
// Parent Component
function Parent() {
  const [message, setMessage] = useState('');

  const handleMessage = (msg) => {
    setMessage(msg);
  };

  return (
    <div>
      <p>Message: {message}</p>
      <Child onSendMessage={handleMessage} />
    </div>
  );
}

// Child Component
function Child({ onSendMessage }) {
  return (
    <button onClick={() => onSendMessage('Hello from child')}>
      Send Message
    </button>
  );
}
```

### 18. What is conditional rendering?
**Answer:** Rendering different content based on conditions.

```jsx
// if-else
function Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  }
  return <h1>Please log in</h1>;
}

// Ternary operator
<h1>{isLoggedIn ? 'Welcome back!' : 'Please log in'}</h1>

// Logical AND (&&)
{isLoggedIn && <h1>Welcome back!</h1>}

// Switch statement
function Status({ status }) {
  switch(status) {
    case 'loading':
      return <p>Loading...</p>;
    case 'success':
      return <p>Success!</p>;
    case 'error':
      return <p>Error!</p>;
    default:
      return null;
  }
}

// Inline object for multiple conditions
const renderContent = {
  loading: () => <p>Loading...</p>,
  success: () => <p>Success!</p>,
  error: () => <p>Error!</p>
};

function Component({ status }) {
  return renderContent[status]?.() || null;
}
```

### 19. What is list rendering?
**Answer:** Rendering multiple items using array methods.

```jsx
function ItemList() {
  const items = ['Apple', 'Banana', 'Orange'];

  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}

// With objects
const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' }
];

{users.map(user => (
  <div key={user.id}>{user.name}</div>
))}

// Filtering
{users
  .filter(user => user.name.includes('J'))
  .map(user => <div key={user.id}>{user.name}</div>)
}
```

---

## Lifecycle Methods

### 20. What are lifecycle methods?
**Answer:** Methods that run at specific points in component lifecycle.

```jsx
class MyComponent extends React.Component {
  // 1. Constructor
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  // 2. Mounting Phase
  componentDidMount() {
    // After component mounted to DOM
    // Good for: API calls, subscriptions, timers
  }

  // 3. Updating Phase
  componentDidUpdate(prevProps, prevState) {
    // After component updated
    // Good for: API calls based on prop changes
  }

  // 4. Unmounting Phase
  componentWillUnmount() {
    // Before component removed from DOM
    // Good for: cleanup, unsubscribe
  }

  render() {
    return <div>Count: {this.state.count}</div>;
  }
}
```

**Lifecycle diagram:**
```
Mounting → Updating → Unmounting
  ↓          ↓          ↓
constructor componentDidMount      componentWillUnmount
  ↓          ↓
render     render
  ↓          ↓
componentDidMount componentDidUpdate
```

### 21. What is shouldComponentUpdate?
**Answer:** Controls whether component should re-render.

```jsx
class MyComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    // Return false to skip re-render
    if (nextProps.count === this.props.count) {
      return false; // Don't re-render
    }
    return true; // Re-render
  }

  render() {
    return <div>{this.props.count}</div>;
  }
}

// PureComponent does shallow comparison automatically
class MyComponent extends React.PureComponent {
  render() {
    return <div>{this.props.count}</div>;
  }
}
```

### 22. What is error boundary?
**Answer:** Components that catch JavaScript errors in child components.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error:', error);
    console.log('Error Info:', errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong</h1>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

**Note:** Error boundaries don't catch:
- Event handlers (use try/catch)
- Async code
- Server-side rendering
- Errors in the error boundary itself

---

## Hooks

### 23. What are hooks?
**Answer:** Hooks are functions that let you use state and other React features in functional components.

**Rules of Hooks:**
1. Only call hooks at top level (not in conditions/loops)
2. Only call hooks from React functions

```jsx
// Hook example
function Counter() {
  const [count, setCount] = useState(0); // Hook

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// WRONG: Don't call hooks conditionally
function BadComponent({ condition }) {
  if (condition) {
    const [state, setState] = useState(0); // WRONG!
  }
}

// RIGHT: Call hooks unconditionally
function GoodComponent({ condition }) {
  const [state, setState] = useState(0);
  
  if (condition) {
    // Use state here
  }
}
```

### 24. What is useState?
**Answer:** Hook for adding state to functional components.

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      
      <input 
        value={name} 
        onChange={(e) => setName(e.target.value)}
      />
    </div>
  );
}

// Lazy initialization
function Component() {
  const [state, setState] = useState(() => {
    const initialValue = someExpensiveComputation();
    return initialValue;
  });
}

// Updating state based on previous state
setState(prevState => prevState + 1);

// Multiple states
const [formData, setFormData] = useState({
  name: '',
  email: '',
  age: 0
});

setFormData(prev => ({
  ...prev,
  name: 'John'
}));
```

### 25. What is useEffect?
**Answer:** Hook for side effects (API calls, subscriptions, timers, etc.).

```jsx
// Run after every render
useEffect(() => {
  document.title = `Clicked ${count} times`;
});

// Run only once (on mount)
useEffect(() => {
  console.log('Component mounted');
}, []);

// Run when dependencies change
useEffect(() => {
  console.log('Count changed:', count);
}, [count]);

// Cleanup function
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Timer');
  }, 1000);

  return () => clearInterval(timer); // Cleanup on unmount
}, []);

// Multiple effects
useEffect(() => { /* effect 1 */ }, [dep1]);
useEffect(() => { /* effect 2 */ }, [dep2]);
```

**Dependency array:**
- No array: Runs after every render
- Empty array: Runs once on mount
- [dep]: Runs when dep changes

### 26. What is useContext?
**Answer:** Hook for consuming context without nesting.

```jsx
// Create context
const ThemeContext = React.createContext();

// Provider (parent)
function App() {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Component />
    </ThemeContext.Provider>
  );
}

// Consumer (child)
function MyComponent() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle
      </button>
    </div>
  );
}
```

### 27. What is useReducer?
**Answer:** Hook for complex state logic.

```jsx
// Reducer function
const initialState = { count: 0 };

function reducer(state, action) {
  switch(action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    case 'RESET':
      return initialState;
    default:
      return state;
  }
}

// Using useReducer
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
    </div>
  );
}

// With payload
function reducer(state, action) {
  switch(action.type) {
    case 'ADD':
      return { count: state.count + action.payload };
    default:
      return state;
  }
}

dispatch({ type: 'ADD', payload: 5 });
```

### 28. What is useRef?
**Answer:** Hook for accessing DOM elements directly and storing mutable values.

```jsx
// Accessing DOM elements
function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}

// Storing mutable values
function Stopwatch() {
  const intervalRef = useRef(null);

  const startTimer = () => {
    intervalRef.current = setInterval(() => {
      // Timer logic
    }, 1000);
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </>
  );
}

// useRef doesn't cause re-render
const countRef = useRef(0);
countRef.current++; // No re-render
```

### 29. What is useCallback?
**Answer:** Memoizes a function to prevent unnecessary re-renders of child components.

```jsx
// Without useCallback
function Parent({ onCallback }) {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log('Clicked');
  };

  // handleClick is recreated on every render
  return (
    <div>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}

// With useCallback
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []); // Empty dependency array

  // handleClick is memoized and only recreated if dependencies change
  return (
    <div>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}

// With dependencies
const handleClick = useCallback(() => {
  console.log(name);
}, [name]); // Recreates when name changes
```

### 30. What is useMemo?
**Answer:** Memoizes expensive computations.

```jsx
// Without useMemo
function Component({ items }) {
  const [sort, setSort] = useState(false);

  // computeExpensive runs on every render
  const sorted = items.sort((a, b) => a - b);

  return (
    <div>
      <p>{sorted}</p>
      <button onClick={() => setSort(!sort)}>Toggle</button>
    </div>
  );
}

// With useMemo
function Component({ items }) {
  const [sort, setSort] = useState(false);

  const sorted = useMemo(() => {
    console.log('Computing...');
    return items.sort((a, b) => a - b);
  }, [items]); // Only recomputes when items changes

  return (
    <div>
      <p>{sorted}</p>
      <button onClick={() => setSort(!sort)}>Toggle</button>
    </div>
  );
}
```

### 31. What is useLayoutEffect?
**Answer:** Similar to useEffect but fires synchronously after DOM mutations.

```jsx
// useEffect: Async, after paint
useEffect(() => {
  console.log('useEffect');
});

// useLayoutEffect: Sync, before paint
useLayoutEffect(() => {
  console.log('useLayoutEffect');
});

// Order: render → useLayoutEffect → paint → useEffect

// Use case: Measuring DOM
useLayoutEffect(() => {
  const rect = ref.current.getBoundingClientRect();
  console.log('Width:', rect.width);
}, []);
```

### 32. What is custom hooks?
**Answer:** Custom hooks are JavaScript functions that use React hooks.

```jsx
// Custom hook for form input
function useInput(initialValue) {
  const [value, setValue] = useState(initialValue);

  return {
    value,
    setValue,
    bind: {
      value,
      onChange: (e) => setValue(e.target.value)
    },
    reset: () => setValue(initialValue)
  };
}

// Using custom hook
function Form() {
  const name = useInput('');
  const email = useInput('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name.value, email.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input {...name.bind} />
      <input {...email.bind} />
      <button>Submit</button>
      <button onClick={name.reset}>Reset</button>
    </form>
  );
}

// Custom hook for API
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}
```

---

## Event Handling

### 33. How do you handle events in React?
**Answer:** Using camelCase event handlers and synthetic events.

```jsx
function Button() {
  const handleClick = () => {
    console.log('Button clicked');
  };

  const handleChange = (e) => {
    console.log('Input value:', e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted');
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
      <input onChange={handleChange} />
      <form onSubmit={handleSubmit}>
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

// Event binding in class components
class Button extends React.Component {
  handleClick = () => {
    console.log(this); // this is bound
  };

  // Or bind in constructor
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  render() {
    return <button onClick={this.handleClick}>Click</button>;
  }
}
```

### 34. What are synthetic events?
**Answer:** React wraps browser events in cross-browser compatible synthetic events.

```jsx
function Form() {
  const handleChange = (e) => {
    // e is synthetic event
    console.log(e.type); // 'change'
    console.log(e.target.value); // input value
    console.log(e.nativeEvent); // Native browser event
  };

  return <input onChange={handleChange} />;
}

// Event pooling (deprecated in React 17)
// In older React versions, events were pooled and reused
```

### 35. How do you pass arguments to event handlers?
**Answer:** Using arrow functions or bind.

```jsx
function List({ items }) {
  const handleDelete = (id) => {
    console.log('Delete', id);
  };

  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          {item.name}
          {/* Arrow function */}
          <button onClick={() => handleDelete(item.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}

// Event object and argument
const handleClick = (id, e) => {
  console.log('ID:', id);
  console.log('Event:', e);
};

<button onClick={(e) => handleClick(1, e)}>Click</button>
```

---

## Forms & Validation

### 36. How do you handle form inputs?
**Answer:** Using controlled components with state.

```jsx
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        name="name" 
        value={formData.name} 
        onChange={handleChange}
      />
      <input 
        name="email" 
        type="email"
        value={formData.email} 
        onChange={handleChange}
      />
      <textarea 
        name="message" 
        value={formData.message} 
        onChange={handleChange}
      />
      <select 
        name="country" 
        value={formData.country} 
        onChange={handleChange}
      >
        <option>Select</option>
        <option>USA</option>
        <option>India</option>
      </select>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### 37. How do you validate forms?
**Answer:** Validating on change or submit.

```jsx
function Form() {
  const [formData, setFormData] = useState({ email: '', password: '' });
  const [errors, setErrors] = useState({});

  const validate = () => {
    const newErrors = {};
    
    if (!formData.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      console.log('Form is valid');
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input 
          name="email" 
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <span>{errors.email}</span>}
      </div>
      <div>
        <input 
          type="password"
          name="password" 
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <span>{errors.password}</span>}
      </div>
      <button>Submit</button>
    </form>
  );
}
```

---

## Performance Optimization

### 38. What are common performance issues?
**Answer:** Re-renders, memory leaks, bundle size.

```jsx
// Problem: Unnecessary re-renders
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <ExpensiveChild /> {/* Re-renders on every parent re-render */}
    </div>
  );
}

// Solution: Memoize child
const ExpensiveChild = React.memo(() => {
  console.log('Rendering expensive child');
  return <div>Expensive Component</div>;
});
```

### 39. What is React.lazy and Suspense?
**Answer:** Code splitting for lazy loading components.

```jsx
// Without lazy
import HeavyComponent from './HeavyComponent';

// With lazy
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}

// Multiple lazy components
function Dashboard() {
  return (
    <Suspense fallback={<div>Loading dashboard...</div>}>
      <div>
        <Suspense fallback={<div>Loading users...</div>}>
          <Users />
        </Suspense>
        <Suspense fallback={<div>Loading posts...</div>}>
          <Posts />
        </Suspense>
      </div>
    </Suspense>
  );
}
```

### 40. What is windowing?
**Answer:** Rendering only visible items in a large list.

```jsx
// Without windowing: renders all 1000 items
function VirtualList({ items }) {
  return (
    <div>
      {items.map(item => <Item key={item.id} item={item} />)}
    </div>
  );
}

// With windowing: uses react-window
import { FixedSizeList } from 'react-window';

function VirtualList({ items }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={35}
      width="100%"
    >
      {({ index, style }) => (
        <Item style={style} item={items[index]} />
      )}
    </FixedSizeList>
  );
}
```

### 41. How do you optimize bundle size?
**Answer:** Code splitting, tree shaking, compression.

```jsx
// Code splitting
const App = lazy(() => import('./App'));

// Dynamic imports
const Dashboard = lazy(() => import(/* webpackChunkName: "dashboard" */ './Dashboard'));

// Tree shaking: Only import what you need
import { map, filter } from 'lodash-es';

// Avoid importing entire libraries
// Bad
import _ from 'lodash';
_.map(arr, item => item.id);

// Good
import map from 'lodash-es/map';
map(arr, item => item.id);
```

---

## Routing

### 42. How do you implement routing?
**Answer:** Using React Router.

```jsx
import { BrowserRouter, Routes, Route, Link, useNavigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/products">Products</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/products/:id" element={<Product />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// Using useNavigate hook
function Product() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/');
  };

  return <button onClick={handleClick}>Go Home</button>;
}

// Using useParams hook
function Product() {
  const { id } = useParams();
  return <div>Product: {id}</div>;
}

// Protected routes
function ProtectedRoute({ children }) {
  const isAuthenticated = true; // Check auth

  return isAuthenticated ? children : <Navigate to="/login" />;
}

<Routes>
  <Route 
    path="/dashboard" 
    element={
      <ProtectedRoute>
        <Dashboard />
      </ProtectedRoute>
    } 
  />
</Routes>
```

### 43. What is lazy loading routes?
**Answer:** Loading route components only when needed.

```jsx
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// Lazy load components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Products = lazy(() => import('./pages/Products'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/products" element={<Products />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

---

## State Management

### 44. What are state management solutions?
**Answer:** Redux, Context API, Zustand, Recoil, MobX.

**Context API:**
```jsx
// Create context
const AuthContext = React.createContext();

// Provider
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

// Consumer
function Profile() {
  const { user, logout } = useContext(AuthContext);
  return <button onClick={logout}>Logout {user?.name}</button>;
}
```

**Redux:**
```jsx
// Action
const INCREMENT = 'INCREMENT';

// Reducer
function counterReducer(state = 0, action) {
  switch(action.type) {
    case INCREMENT:
      return state + 1;
    default:
      return state;
  }
}

// Store
import { createStore } from 'redux';
const store = createStore(counterReducer);

// Component
function Counter() {
  const count = useSelector(state => state);
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch({ type: INCREMENT })}>+</button>
    </div>
  );
}
```

### 45. What is Redux and when to use it?
**Answer:** Redux is state management for complex apps.

**When to use Redux:**
- Large app with complex state
- State needed by many components
- Frequent state updates
- Time-travel debugging needed

**When NOT to use:**
- Simple app
- Local component state sufficient
- Learning React

```jsx
// Redux Toolkit (modern approach)
import { createSlice, configureStore } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    }
  }
});

const store = configureStore({
  reducer: counterSlice.reducer
});

// In component
const count = useSelector(state => state.value);
const dispatch = useDispatch();

<button onClick={() => dispatch(counterSlice.actions.increment())}>
  Increment
</button>
```

---

## Advanced Topics

### 46. What is server-side rendering (SSR)?
**Answer:** Rendering React on server instead of client.

**Benefits:**
- Better SEO
- Faster initial load
- Better performance on slow devices

```jsx
// Server
import { renderToString } from 'react-dom/server';

app.get('/', (req, res) => {
  const html = renderToString(<App />);
  res.send(html);
});

// Client
import { createRoot, hydrateRoot } from 'react-dom/client';

hydrateRoot(document.getElementById('root'), <App />);
```

### 47. What is Next.js?
**Answer:** React framework with built-in SSR, static generation, and routing.

```jsx
// pages/index.js
export default function Home() {
  return <h1>Welcome to Next.js</h1>;
}

// pages/api/hello.js (API route)
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello' });
}

// getStaticProps (static generation)
export async function getStaticProps() {
  return {
    props: {
      data: 'static data'
    }
  };
}

// getServerSideProps (SSR)
export async function getServerSideProps() {
  return {
    props: {
      data: 'server data'
    }
  };
}
```

### 48. What is concurrent rendering?
**Answer:** React's ability to interrupt rendering for higher-priority updates.

```jsx
// Automatic batching
function Component() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(c => c + 1); // Batched
    setCount(c => c + 1); // Batched
    // Renders once, not twice
  };

  return <button onClick={handleClick}>Count: {count}</button>;
}

// useTransition for non-blocking updates
function SearchComponent() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    const value = e.target.value;
    setQuery(value); // Urgent update

    startTransition(() => {
      // Non-urgent update
      performSearch(value);
    });
  };

  return (
    <div>
      <input onChange={handleChange} value={query} />
      {isPending && <div>Searching...</div>}
    </div>
  );
}
```

### 49. What is Suspense?
**Answer:** Component for handling async data loading.

```jsx
// Data fetching with Suspense
const resource = fetchUserData(userId);

function User() {
  const user = resource.read(); // Throws promise
  return <div>{user.name}</div>;
}

function App() {
  return (
    <Suspense fallback={<div>Loading user...</div>}>
      <User />
    </Suspense>
  );
}

// Server components
async function ServerComponent() {
  const data = await fetchData();
  return <div>{data}</div>;
}
```

### 50. What is strict mode warnings?
**Answer:** Development-only checks for unsafe code.

Common warnings:
- Unsafe lifecycles
- Legacy string refs
- Unexpected side effects
- Unsafe context API

```jsx
// Fix: Component should be pure (no side effects in render)
// Bad
function Component() {
  global.count++; // Side effect
  return <div>{global.count}</div>;
}

// Good
function Component() {
  const [count, setCount] = useState(0);
  return <div>{count}</div>;
}
```

---

## Common Interview Tricky Questions

### 51. What will be the output?
```jsx
function Component() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => {
      setCount(count + 1);
      setCount(count + 1);
      setCount(count + 1);
    }}>
      Count: {count}
    </button>
  );
}

// Answer: Count becomes 1 (not 3)
// setState is batched and the count closure still holds 0
```

### 52. What about this?
```jsx
useEffect(() => {
  console.log('Effect');
  return () => console.log('Cleanup');
}, []);

// Output:
// Effect
// (on unmount) Cleanup
```

### 53. What about closure?
```jsx
function Component() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    console.log(count); // What will it log?
  }, []); // Missing count dependency

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <button onClick={increment}>Log Count</button>
    </div>
  );
}

// Answer: Always logs 0
// Fix: Add [count] to dependency array
```

### 54. What about ref update?
```jsx
function Component() {
  const ref = useRef(0);

  const handleClick = () => {
    ref.current++;
    console.log(ref.current); // Logs: 1, 2, 3...
  };

  return <button onClick={handleClick}>Click</button>;
}

// ref.current++ doesn't trigger re-render
```

### 55. What about multiple state updates?
```jsx
function Component() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  const handleClick = async () => {
    setCount(count + 1);
    setFlag(!flag);
    
    // Both setState calls are batched
    // Component renders once with new count and flag
  };

  return <button onClick={handleClick}>Update</button>;
}
```

---

## Comparison with Other Frameworks

### 56. React vs Vue vs Angular

| Feature | React | Vue | Angular |
|---------|-------|-----|---------|
| Learning Curve | Medium | Easy | Hard |
| Bundle Size | ~40KB | ~35KB | ~150KB |
| Performance | Excellent | Excellent | Good |
| State Management | Flexible | Built-in | Built-in |
| Ecosystem | Largest | Medium | Large |
| Typing | Optional | Optional | Required (TS) |
| Syntax | JSX | Templates | TypeScript |

---

## Interview Tips

1. **Understand concepts deeply** - Not just syntax
2. **Know when to use what** - When to use hooks vs class components
3. **Performance matters** - Know optimization techniques
4. **Read error messages** - They often tell you what's wrong
5. **Test your code** - Write tests to verify behavior
6. **Keep up with updates** - React evolves
7. **Practice building apps** - Not just understanding concepts
8. **Explain your thinking** - Communicate clearly
9. **Know the ecosystem** - Redux, React Router, Next.js, etc.
10. **Mock interviews** - Practice under pressure

---

## Helpful Resources

- **React Docs**: https://react.dev/
- **React GitHub**: https://github.com/facebook/react
- **Create React App**: https://create-react-app.dev/
- **Next.js**: https://nextjs.org/
- **React Router**: https://reactrouter.com/
- **Redux**: https://redux.js.org/
- **React Testing Library**: https://testing-library.com/
- **Storybook**: https://storybook.js.org/
- **Dev.to React**: https://dev.to/t/react
- **React Patterns**: https://reactpatterns.com/

---

## Practice Projects

1. **Todo App** - Learn state and basic hooks
2. **Weather App** - Learn API calls and useEffect
3. **E-commerce Site** - Learn routing and state management
4. **Blog Platform** - Learn authentication and CRUD
5. **Social Media** - Learn complex state and real-time updates

Good luck with your React interviews! 🚀

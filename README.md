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

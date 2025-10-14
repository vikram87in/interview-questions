# JavaScript Interview Questions

## Variables (var, let, const)

**Beginner: Q1** - What is the difference between `var`, `let`, and `const` in JavaScript?

**Beginner: Q2** - Can you reassign values to variables declared with `const`? What about objects and arrays?

**Beginner: Q3** - What will happen if you try to access a variable before declaring it with `let` or `const`?

**Intermediate: Q1** - Explain the temporal dead zone and how it relates to `let` and `const`.

**Intermediate: Q2** - What is the output of this code?
```javascript
var x = 1;
function test() {
    console.log(x);
    var x = 2;
    console.log(x);
}
test();
```

## Hoisting

**Beginner: Q1** - What is hoisting in JavaScript? Give an example.

**Beginner: Q2** - Are function declarations hoisted? What about function expressions?

**Beginner: Q3** - What will be the output of this code?
```javascript
console.log(myVar);
var myVar = 5;
```

**Intermediate: Q1** - Explain the difference in hoisting behavior between `var`, `let`, `const`, and function declarations.

**Intermediate: Q2** - What is the output and why?
```javascript
console.log(typeof foo);
console.log(typeof bar);
var foo = 'hello';
function bar() { return 'world'; }
```

## Scope (global, function, block)

**Beginner: Q1** - What are the different types of scope in JavaScript?

**Beginner: Q2** - What is the difference between global scope and function scope?

**Beginner: Q3** - Do `var` declarations have block scope? What about `let` and `const`?

**Intermediate: Q1** - What will be the output of this code and why?
```javascript
var i = 0;
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```

**Intermediate: Q2** - How would you fix the above code to print 0, 1, 2 instead?

## Function declarations vs expressions

**Beginner: Q1** - What is the difference between a function declaration and a function expression?

**Beginner: Q2** - Which of these can be called before it's defined in the code?

**Beginner: Q3** - What is an anonymous function expression?

**Intermediate: Q1** - What will be the output of this code?
```javascript
console.log(foo());
console.log(bar());

function foo() { return 'foo'; }
var bar = function() { return 'bar'; };
```

**Intermediate: Q2** - What is a named function expression and when would you use it?

## First-class functions

**Beginner: Q1** - What does it mean that functions are "first-class citizens" in JavaScript?

**Beginner: Q2** - Can you assign a function to a variable? Can you pass a function as an argument?

**Beginner: Q3** - Can functions be returned from other functions?

**Intermediate: Q1** - Write a function that takes another function as a parameter and executes it twice.

**Intermediate: Q2** - How would you store functions in an array and execute them?

## Higher-order functions

**Beginner: Q1** - What is a higher-order function? Give an example.

**Beginner: Q2** - Name three built-in higher-order functions in JavaScript arrays.

**Beginner: Q3** - What does the `map()` function do?

**Intermediate: Q1** - Implement your own version of the `filter()` function.

**Intermediate: Q2** - What is the difference between `map()`, `forEach()`, and `filter()`?

## Closures

**Beginner: Q1** - What is a closure in JavaScript?

**Beginner: Q2** - Give a simple example of a closure.

**Beginner: Q3** - Why do closures have access to outer function variables even after the outer function returns?

**Intermediate: Q1** - What will be the output of this code?
```javascript
function outer() {
    var x = 10;
    return function inner() {
        console.log(x);
        x++;
    };
}
var fn = outer();
fn();
fn();
```

**Intermediate: Q2** - How can closures lead to memory leaks? How would you prevent them?

## IIFE (Immediately Invoked Function Expressions)

**Beginner: Q1** - What is an IIFE? Write the syntax for one.

**Beginner: Q2** - Why would you use an IIFE?

**Beginner: Q3** - Can you pass parameters to an IIFE?

**Intermediate: Q1** - How were IIFEs used to create modules before ES6 modules?

**Intermediate: Q2** - What's the difference between these two IIFE syntaxes?
```javascript
(function() { })();
(function() { }());
```

## Recursion

**Beginner: Q1** - What is recursion? Write a simple recursive function to calculate factorial.

**Beginner: Q2** - What are the two essential parts of a recursive function?

**Beginner: Q3** - What happens if you forget the base case in recursion?

**Intermediate: Q1** - Write a recursive function to flatten a nested array.

**Intermediate: Q2** - What is tail recursion and how does it help with performance?

## Currying

**Beginner: Q1** - What is currying in JavaScript?

**Beginner: Q2** - Convert this function to a curried version: `function add(a, b, c) { return a + b + c; }`

**Beginner: Q3** - What is the advantage of currying?

**Intermediate: Q1** - Implement a generic curry function that can curry any function.

**Intermediate: Q2** - What is partial application and how is it different from currying?

## Pure vs impure functions

**Beginner: Q1** - What is a pure function? Give an example.

**Beginner: Q2** - What is an impure function? Give an example.

**Beginner: Q3** - What are the benefits of using pure functions?

**Intermediate: Q1** - Is this function pure or impure? Explain why.
```javascript
let counter = 0;
function increment() {
    return ++counter;
}
```

**Intermediate: Q2** - How do pure functions relate to functional programming concepts like immutability?

## Object creation ({}, Object.create, classes)

**Beginner: Q1** - What are the different ways to create objects in JavaScript?

**Beginner: Q2** - What is the difference between object literal syntax and using `new Object()`?

**Beginner: Q3** - How do you create an object using a constructor function?

**Intermediate: Q1** - What is `Object.create()` and how is it different from other object creation methods?

**Intermediate: Q2** - Compare ES6 classes with constructor functions for object creation.

## Prototype chain & inheritance

**Beginner: Q1** - What is the prototype chain in JavaScript?

**Beginner: Q2** - How do you add a method to all instances of a constructor function?

**Beginner: Q3** - What happens when you try to access a property that doesn't exist on an object?

**Intermediate: Q1** - Explain prototypal inheritance with an example.

**Intermediate: Q2** - What will be the output of this code?
```javascript
function Animal() {}
Animal.prototype.speak = function() { return 'Animal speaks'; };

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.speak = function() { return 'Dog barks'; };

const dog = new Dog();
console.log(dog.speak());
delete Dog.prototype.speak;
console.log(dog.speak());
```

## __proto__ vs prototype

**Beginner: Q1** - What is the difference between `__proto__` and `prototype`?

**Beginner: Q2** - When you create an object with `new`, what does `__proto__` point to?

**Beginner: Q3** - Is `__proto__` a standard property or should you avoid using it?

**Intermediate: Q1** - Explain the relationship between `__proto__`, `prototype`, and `constructor`.

**Intermediate: Q2** - What is the difference between `Object.getPrototypeOf()` and accessing `__proto__`?

## this keyword

**Beginner: Q1** - What does the `this` keyword refer to in different contexts?

**Beginner: Q2** - What will `this` refer to inside a regular function vs an arrow function?

**Beginner: Q3** - How does the value of `this` change when a method is called vs when it's assigned to a variable?

**Intermediate: Q1** - What will be the output of this code?
```javascript
const obj = {
    name: 'John',
    greet: function() {
        console.log(this.name);
    },
    greetArrow: () => {
        console.log(this.name);
    }
};
obj.greet();
obj.greetArrow();
```

**Intermediate: Q2** - How does `this` behave in strict mode vs non-strict mode?

## call, apply, bind

**Beginner: Q1** - What is the difference between `call()`, `apply()`, and `bind()`?

**Beginner: Q2** - How do you use `call()` to invoke a function with a specific `this` value?

**Beginner: Q3** - When would you use `bind()` instead of `call()` or `apply()`?

**Intermediate: Q1** - Implement your own version of the `bind()` method.

**Intermediate: Q2** - What will be the output of this code?
```javascript
function greet() {
    console.log(`${this.greeting} ${this.name}`);
}
const person = { greeting: 'Hello', name: 'John' };
const boundGreet = greet.bind(person);
boundGreet.call({ greeting: 'Hi', name: 'Jane' });
```

## Object.keys, values, entries

**Beginner: Q1** - What do `Object.keys()`, `Object.values()`, and `Object.entries()` return?

**Beginner: Q2** - How would you iterate over an object's properties using these methods?

**Beginner: Q3** - Do these methods include inherited properties?

**Intermediate: Q1** - What's the difference between `for...in` loop and `Object.keys()`?

**Intermediate: Q2** - How would you convert an array of key-value pairs back to an object?

## Object.freeze, seal, assign

**Beginner: Q1** - What does `Object.freeze()` do? Can you modify a frozen object?

**Beginner: Q2** - What is the difference between `Object.freeze()` and `Object.seal()`?

**Beginner: Q3** - How do you copy properties from one object to another?

**Intermediate: Q1** - What will happen in this code?
```javascript
const obj = { a: 1, b: { c: 2 } };
Object.freeze(obj);
obj.b.c = 3;
console.log(obj.b.c);
```

**Intermediate: Q2** - How is `Object.assign()` different from the spread operator for object copying?

## Shallow vs deep copy

**Beginner: Q1** - What is the difference between shallow copy and deep copy?

**Beginner: Q2** - Give an example where shallow copying would cause issues.

**Beginner: Q3** - How would you create a shallow copy of an object?

**Intermediate: Q1** - Implement a function to create a deep copy of an object.

**Intermediate: Q2** - What are the limitations of using `JSON.parse(JSON.stringify())` for deep copying?

## Arrow functions

**Beginner: Q1** - What is the syntax for arrow functions? Give examples of different forms.

**Beginner: Q2** - How do arrow functions handle the `this` keyword differently from regular functions?

**Beginner: Q3** - Can arrow functions be used as constructors?

**Intermediate: Q1** - What will be the output of this code?
```javascript
const obj = {
    name: 'Test',
    getName: function() {
        const inner = () => {
            return this.name;
        };
        return inner();
    }
};
console.log(obj.getName());
```

**Intermediate: Q2** - When should you use arrow functions and when should you avoid them?

## Default/rest/spread parameters

**Beginner: Q1** - How do you set default values for function parameters in ES6?

**Beginner: Q2** - What are rest parameters and how do you use them?

**Beginner: Q3** - What is the spread operator and how is it used with arrays?

**Intermediate: Q1** - What will be the output of this code?
```javascript
function test(a = 1, b = a + 1, c = b + a) {
    return [a, b, c];
}
console.log(test(5));
console.log(test());
```

**Intermediate: Q2** - How can you use the spread operator to merge objects and arrays?

## Destructuring (arrays, objects)

**Beginner: Q1** - How do you destructure arrays and objects in JavaScript?

**Beginner: Q2** - How do you assign default values during destructuring?

**Beginner: Q3** - How do you rename variables during object destructuring?

**Intermediate: Q1** - What will be the output of this code?
```javascript
const arr = [1, 2, 3, 4, 5];
const [first, , third, ...rest] = arr;
console.log(first, third, rest);
```

**Intermediate: Q2** - How do you destructure nested objects and arrays?

## Template literals

**Beginner: Q1** - What are template literals and how are they different from regular strings?

**Beginner: Q2** - How do you embed expressions in template literals?

**Beginner: Q3** - Can template literals span multiple lines?

**Intermediate: Q1** - What are tagged template literals and how do they work?

**Intermediate: Q2** - Write a tagged template function that highlights variables in a string.

## Modules (import, export)

**Beginner: Q1** - How do you export and import functions from modules in ES6?

**Beginner: Q2** - What is the difference between named exports and default exports?

**Beginner: Q3** - How do you import all exports from a module?

**Intermediate: Q1** - What is dynamic import and when would you use it?

**Intermediate: Q2** - How do ES6 modules differ from CommonJS modules?

## Classes & inheritance

**Beginner: Q1** - How do you create a class in ES6? How do you create an instance?

**Beginner: Q2** - How do you implement inheritance using the `extends` keyword?

**Beginner: Q3** - What is the `super` keyword used for?

**Intermediate: Q1** - What will be the output of this code?
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    speak() {
        return `${super.speak()} - Woof!`;
    }
}

const dog = new Dog('Buddy');
console.log(dog.speak());
```

**Intermediate: Q2** - How do private fields work in ES6 classes?

## Async/await

**Beginner: Q1** - What is `async/await` and how is it used?

**Beginner: Q2** - How do you handle errors in async/await?

**Beginner: Q3** - Can you use `await` without `async`?

**Intermediate: Q1** - What will be the output of this code?
```javascript
async function test() {
    console.log('1');
    await Promise.resolve();
    console.log('2');
}
console.log('3');
test();
console.log('4');
```

**Intermediate: Q2** - How do you handle multiple async operations concurrently with async/await?

## Promises

**Beginner: Q1** - What is a Promise and what are its three states?

**Beginner: Q2** - How do you create a Promise and handle its result?

**Beginner: Q3** - What is the difference between `.then()` and `.catch()`?

**Intermediate: Q1** - What is the difference between `Promise.all()`, `Promise.race()`, and `Promise.allSettled()`?

**Intermediate: Q2** - What will be the output of this code?
```javascript
Promise.resolve(1)
    .then(x => x + 1)
    .then(x => { throw new Error('Error!'); })
    .then(x => x + 1)
    .catch(err => 5)
    .then(x => x + 1);
```

## Event loop & call stack

**Beginner: Q1** - What is the call stack in JavaScript?

**Beginner: Q2** - What is the event loop and how does it work?

**Beginner: Q3** - What happens when the call stack is empty?

**Intermediate: Q1** - What will be the output of this code?
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
console.log('3');
Promise.resolve().then(() => console.log('4'));
console.log('5');
```

**Intermediate: Q2** - Explain the relationship between the call stack, callback queue, and microtask queue.

## Callback functions

**Beginner: Q1** - What is a callback function? Give an example.

**Beginner: Q2** - Why are callbacks used in asynchronous operations?

**Beginner: Q3** - What is a callback passed to `setTimeout`?

**Intermediate: Q1** - How do you handle errors in callback-based functions?

**Intermediate: Q2** - What are the problems with deeply nested callbacks?

## Callback hell

**Beginner: Q1** - What is callback hell?

**Beginner: Q2** - Why is callback hell considered a problem?

**Beginner: Q3** - Give an example of callback hell.

**Intermediate: Q1** - How can Promises help solve callback hell?

**Intermediate: Q2** - What are other strategies to avoid callback hell?

## Promises & chaining

**Beginner: Q1** - How do you chain multiple Promises together?

**Beginner: Q2** - What does each `.then()` return?

**Beginner: Q3** - How do you handle errors in a Promise chain?

**Intermediate: Q1** - What happens if you don't return anything from a `.then()` handler?

**Intermediate: Q2** - How do you convert callback-based functions to Promises?

## Microtasks vs macrotasks

**Beginner: Q1** - What are microtasks and macrotasks?

**Beginner: Q2** - Which has higher priority: microtasks or macrotasks?

**Beginner: Q3** - Give examples of microtasks and macrotasks.

**Intermediate: Q1** - What will be the execution order of this code?
```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
console.log('end');
```

**Intermediate: Q2** - How does the event loop process microtasks and macrotasks?

## setTimeout, setInterval

**Beginner: Q1** - What is the difference between `setTimeout` and `setInterval`?

**Beginner: Q2** - How do you cancel a timeout or interval?

**Beginner: Q3** - What is the minimum delay you can set with `setTimeout`?

**Intermediate: Q1** - What are the potential issues with `setInterval`? How would you fix them?

**Intermediate: Q2** - What will be the output of this code?
```javascript
for (var i = 1; i <= 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```

## Web APIs

**Beginner: Q1** - What are Web APIs in the context of JavaScript?

**Beginner: Q2** - Name some common Web APIs available in browsers.

**Beginner: Q3** - Are Web APIs part of the JavaScript language itself?

**Intermediate: Q1** - How do Web APIs interact with the JavaScript event loop?

**Intermediate: Q2** - What is the difference between browser APIs and Node.js APIs?

## Execution context

**Beginner: Q1** - What is an execution context in JavaScript?

**Beginner: Q2** - What are the different types of execution contexts?

**Beginner: Q3** - When is a new execution context created?

**Intermediate: Q1** - What are the phases of execution context creation?

**Intermediate: Q2** - How does the execution context stack work?

## Lexical environment

**Beginner: Q1** - What is a lexical environment?

**Beginner: Q2** - How is lexical environment related to scope?

**Beginner: Q3** - What happens to variables in a lexical environment?

**Intermediate: Q1** - Explain the relationship between lexical environment and closures.

**Intermediate: Q2** - How do `let` and `const` create temporal dead zones in lexical environments?

## Scope chain

**Beginner: Q1** - What is the scope chain?

**Beginner: Q2** - How does JavaScript resolve variable names using the scope chain?

**Beginner: Q3** - What happens when a variable is not found in the current scope?

**Intermediate: Q1** - How does the scope chain relate to lexical scoping?

**Intermediate: Q2** - What will be the output of this code?
```javascript
var x = 'global';
function outer() {
    var x = 'outer';
    function inner() {
        console.log(x);
    }
    return inner;
}
outer()();
```

## Garbage collection

**Beginner: Q1** - What is garbage collection in JavaScript?

**Beginner: Q2** - When does garbage collection happen?

**Beginner: Q3** - What types of values are eligible for garbage collection?

**Intermediate: Q1** - What is the mark-and-sweep algorithm?

**Intermediate: Q2** - How can circular references cause memory leaks in older JavaScript engines?

## Memoization

**Beginner: Q1** - What is memoization?

**Beginner: Q2** - When would you use memoization?

**Beginner: Q3** - Give a simple example of memoization.

**Intermediate: Q1** - Implement a generic memoization function.

**Intermediate: Q2** - What are the trade-offs of using memoization?

## Debouncing & throttling

**Beginner: Q1** - What is debouncing? When would you use it?

**Beginner: Q2** - What is throttling? When would you use it?

**Beginner: Q3** - What is the difference between debouncing and throttling?

**Intermediate: Q1** - Implement a debounce function.

**Intermediate: Q2** - Implement a throttle function.

## Polyfills

**Beginner: Q1** - What is a polyfill?

**Beginner: Q2** - Why are polyfills needed?

**Beginner: Q3** - Give an example of when you might need a polyfill.

**Intermediate: Q1** - Write a polyfill for `Array.prototype.includes()`.

**Intermediate: Q2** - How do you detect if a polyfill is needed before adding it?

## Event delegation

**Beginner: Q1** - What is event delegation?

**Beginner: Q2** - Why is event delegation useful?

**Beginner: Q3** - How do you implement event delegation?

**Intermediate: Q1** - What are the advantages and disadvantages of event delegation?

**Intermediate: Q2** - How does event delegation work with dynamically added elements?

## Module patterns (CommonJS, ES Modules, AMD, UMD)

**Beginner: Q1** - What are the different module systems in JavaScript?

**Beginner: Q2** - How do you export and import in CommonJS?

**Beginner: Q3** - What is the difference between CommonJS and ES Modules?

**Intermediate: Q1** - What is UMD and why was it created?

**Intermediate: Q2** - How do you create a module that works in both Node.js and browsers?

## DOM manipulation (querySelector, createElement, etc.)

**Beginner: Q1** - How do you select elements from the DOM?

**Beginner: Q2** - How do you create and append new elements to the DOM?

**Beginner: Q3** - What is the difference between `innerHTML`, `innerText`, and `textContent`?

**Intermediate: Q1** - How do you efficiently manipulate multiple DOM elements?

**Intermediate: Q2** - What is the difference between `querySelector` and `getElementById` in terms of performance?

## Event bubbling & capturing

**Beginner: Q1** - What is event bubbling?

**Beginner: Q2** - What is event capturing?

**Beginner: Q3** - How do you stop event bubbling?

**Intermediate: Q1** - What will happen in this code?
```javascript
document.body.addEventListener('click', () => console.log('body'), true);
document.getElementById('button').addEventListener('click', () => console.log('button'));
// User clicks the button
```

**Intermediate: Q2** - What is the difference between `stopPropagation()` and `stopImmediatePropagation()`?

## Event listeners & passive events

**Beginner: Q1** - How do you add and remove event listeners?

**Beginner: Q2** - What is the difference between `addEventListener` and inline event handlers?

**Beginner: Q3** - Can you add multiple event listeners to the same element?

**Intermediate: Q1** - What are passive event listeners and when would you use them?

**Intermediate: Q2** - How do you prevent the default behavior of an event?

## LocalStorage, SessionStorage, Cookies

**Beginner: Q1** - What are the differences between localStorage, sessionStorage, and cookies?

**Beginner: Q2** - How do you store and retrieve data from localStorage?

**Beginner: Q3** - What happens to sessionStorage when the browser tab is closed?

**Intermediate: Q1** - What are the storage limits for each storage mechanism?

**Intermediate: Q2** - How do you handle storage events and synchronize data across tabs?

## Fetch API / XMLHttpRequest

**Beginner: Q1** - How do you make an HTTP request using the Fetch API?

**Beginner: Q2** - How do you handle the response from a fetch request?

**Beginner: Q3** - What is the difference between Fetch API and XMLHttpRequest?

**Intermediate: Q1** - How do you handle different types of errors in fetch requests?

**Intermediate: Q2** - How do you send POST requests with JSON data using fetch?

## CORS

**Beginner: Q1** - What is CORS and why does it exist?

**Beginner: Q2** - What is a CORS error and when do you encounter it?

**Beginner: Q3** - How do you enable CORS on the server side?

**Intermediate: Q1** - What are preflight requests and when do they occur?

**Intermediate: Q2** - How do you handle credentials in CORS requests?

## WebSockets

**Beginner: Q1** - What are WebSockets and how are they different from HTTP?

**Beginner: Q2** - How do you create a WebSocket connection?

**Beginner: Q3** - What events can you listen for on a WebSocket?

**Intermediate: Q1** - How do you handle WebSocket reconnection and error recovery?

**Intermediate: Q2** - When would you choose WebSockets over HTTP polling?

## Service Workers

**Beginner: Q1** - What are Service Workers and what are they used for?

**Beginner: Q2** - How do you register a Service Worker?

**Beginner: Q3** - What is the lifecycle of a Service Worker?

**Intermediate: Q1** - How do you implement caching strategies with Service Workers?

**Intermediate: Q2** - How do Service Workers enable offline functionality?

## try/catch/finally

**Beginner: Q1** - How do you handle errors using try/catch/finally?

**Beginner: Q2** - When does the finally block execute?

**Beginner: Q3** - Can you have try/catch without finally, or try/finally without catch?

**Intermediate: Q1** - What will be the output of this code?
```javascript
function test() {
    try {
        return 'try';
    } catch (e) {
        return 'catch';
    } finally {
        return 'finally';
    }
}
console.log(test());
```

**Intermediate: Q2** - How do you handle async errors in try/catch blocks?

## Custom errors

**Beginner: Q1** - How do you create and throw a custom error?

**Beginner: Q2** - How do you extend the built-in Error class?

**Beginner: Q3** - What properties should a custom error have?

**Intermediate: Q1** - How do you create different types of custom errors with specific handling?

**Intermediate: Q2** - How do you preserve stack traces in custom errors?

## Error objects

**Beginner: Q1** - What properties does an Error object have?

**Beginner: Q2** - How do you access the error message and stack trace?

**Beginner: Q3** - What are the different types of built-in error objects?

**Intermediate: Q1** - How do you parse and analyze stack traces programmatically?

**Intermediate: Q2** - How do different browsers handle Error objects differently?

## Creating regex (/pattern/ vs new RegExp)

**Beginner: Q1** - What are the two ways to create a regular expression in JavaScript?

**Beginner: Q2** - When would you use `new RegExp()` instead of literal syntax?

**Beginner: Q3** - How do you add flags to a regular expression?

**Intermediate: Q1** - What's the difference between these two regex creations in terms of performance?

**Intermediate: Q2** - How do you create a regex with dynamic patterns?

## Common regex methods (test, match, replace)

**Beginner: Q1** - What does the `test()` method do and what does it return?

**Beginner: Q2** - How do you use `match()` to find patterns in a string?

**Beginner: Q3** - How do you replace text using regular expressions?

**Intermediate: Q1** - What's the difference between `match()` and `exec()` methods?

**Intermediate: Q2** - How do you use capture groups in regex replacement?

## Memory leaks

**Beginner: Q1** - What is a memory leak in JavaScript?

**Beginner: Q2** - What are common causes of memory leaks?

**Beginner: Q3** - How can event listeners cause memory leaks?

**Intermediate: Q1** - How do closures potentially cause memory leaks?

**Intermediate: Q2** - How do you detect and debug memory leaks in a web application?

## Performance optimization techniques

**Beginner: Q1** - What are some basic techniques to optimize JavaScript performance?

**Beginner: Q2** - Why should you minimize DOM manipulation?

**Beginner: Q3** - How does code minification help with performance?

**Intermediate: Q1** - What is lazy loading and how can it improve performance?

**Intermediate: Q2** - How do you optimize loops and iterations for better performance?
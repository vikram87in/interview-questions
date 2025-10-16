# JavaScript Interview Questions

## Variables (var, let, const)

**Beginner: Q1** - What is the difference between `var`, `let`, and `const` in JavaScript?

<details>
<summary>Answer</summary>

- **`var`**: Function-scoped, hoisted, can be redeclared and reassigned
- **`let`**: Block-scoped, hoisted but not initialized, can be reassigned but not redeclared
- **`const`**: Block-scoped, hoisted but not initialized, cannot be reassigned or redeclared

```javascript
var a = 1;
let b = 2;
const c = 3;

// var can be redeclared
var a = 4; // OK

// let cannot be redeclared in same scope
// let b = 5; // SyntaxError

// const cannot be reassigned
// c = 6; // TypeError
```
</details>

**Beginner: Q2** - Can you reassign values to variables declared with `const`? What about objects and arrays?

<details>
<summary>Answer</summary>

No, you cannot reassign `const` variables, but you can modify the contents of objects and arrays:

```javascript
const obj = { name: 'John' };
const arr = [1, 2, 3];

// Cannot reassign
// obj = {}; // TypeError
// arr = []; // TypeError

// Can modify contents
obj.name = 'Jane'; // OK
obj.age = 25; // OK
arr.push(4); // OK
arr[0] = 10; // OK
```
</details>

**Beginner: Q3** - What will happen if you try to access a variable before declaring it with `let` or `const`?

<details>
<summary>Answer</summary>

You'll get a `ReferenceError` due to the temporal dead zone:

```javascript
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 5;

console.log(y); // ReferenceError: Cannot access 'y' before initialization
const y = 10;

// Compare with var:
console.log(z); // undefined (hoisted but not initialized)
var z = 15;
```
</details>

**Intermediate: Q1** - Explain the temporal dead zone and how it relates to `let` and `const`.

<details>
<summary>Answer</summary>

The temporal dead zone (TDZ) is the period between entering a scope and the variable declaration being reached. During this time, the variable exists but cannot be accessed:

```javascript
function example() {
    // TDZ starts here for 'temp'
    console.log(temp); // ReferenceError
    
    let temp = 'Hello'; // TDZ ends here
    console.log(temp); // 'Hello'
}

// TDZ also applies in parameters
function test(a = b, b = 2) {
    return [a, b];
}
test(); // ReferenceError: Cannot access 'b' before initialization
```
</details>

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

<details>
<summary>Answer</summary>

Output:
```
undefined
2
```

Due to hoisting, the function is interpreted as:
```javascript
var x = 1;
function test() {
    var x; // hoisted declaration
    console.log(x); // undefined
    x = 2; // assignment
    console.log(x); // 2
}
```
</details>

## Hoisting

**Beginner: Q1** - What is hoisting in JavaScript? Give an example.

<details>
<summary>Answer</summary>

Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their scope during compilation. Only declarations are hoisted, not initializations:

```javascript
// What you write:
console.log(myVar); // undefined (not ReferenceError)
var myVar = 5;
sayHello(); // "Hello!" (works)

function sayHello() {
    console.log("Hello!");
}

// How JavaScript interprets it:
var myVar; // declaration hoisted
function sayHello() { // function declaration hoisted
    console.log("Hello!");
}
console.log(myVar); // undefined
myVar = 5; // initialization stays in place
```
</details>

**Beginner: Q2** - Are function declarations hoisted? What about function expressions?

<details>
<summary>Answer</summary>

- **Function declarations**: Fully hoisted (can be called before declaration)
- **Function expressions**: Variable is hoisted, but function assignment is not

```javascript
// Function declaration - fully hoisted
sayHello(); // Works: "Hello!"
function sayHello() {
    console.log("Hello!");
}

// Function expression - only variable hoisted
sayGoodbye(); // TypeError: sayGoodbye is not a function
var sayGoodbye = function() {
    console.log("Goodbye!");
};

// Arrow function - same as function expression
sayHi(); // TypeError: sayHi is not a function
var sayHi = () => console.log("Hi!");
```
</details>

**Beginner: Q3** - What will be the output of this code?

```javascript
console.log(myVar);
var myVar = 5;
```

<details>
<summary>Answer</summary>

Output: `undefined`

Due to hoisting, the code is interpreted as:
```javascript
var myVar; // declaration hoisted
console.log(myVar); // undefined
myVar = 5; // initialization stays here
```

The variable is declared but not yet initialized when accessed.
</details>

**Intermediate: Q1** - Explain the difference in hoisting behavior between `var`, `let`, `const`, and function declarations.

<details>
<summary>Answer</summary>

| Type | Hoisted? | Initialized? | Accessible before declaration? |
|------|----------|--------------|------------------------------|
| `var` | Yes | with `undefined` | Yes (returns `undefined`) |
| `let` | Yes | No | No (ReferenceError - TDZ) |
| `const` | Yes | No | No (ReferenceError - TDZ) |
| Function declarations | Yes | Fully | Yes (function works) |

```javascript
// var - hoisted and initialized
console.log(a); // undefined
var a = 1;

// let/const - hoisted but not initialized (TDZ)
console.log(b); // ReferenceError
let b = 2;

// Function declaration - fully hoisted
foo(); // "Hello" - works
function foo() { console.log("Hello"); }
```
</details>

**Intermediate: Q2** - What is the output and why?

```javascript
console.log(typeof foo);
console.log(typeof bar);
var foo = 'hello';
function bar() { return 'world'; }
```

<details>
<summary>Answer</summary>

Output:
```
function
undefined
```

Explanation:
- `bar` is a function declaration, so it's fully hoisted - `typeof bar` returns `"function"`
- `foo` is a `var` declaration, hoisted but initialized with `undefined` - `typeof foo` returns `"undefined"`

The code is interpreted as:
```javascript
var foo; // hoisted, initialized with undefined
function bar() { return 'world'; } // fully hoisted

console.log(typeof foo); // "undefined" 
console.log(typeof bar); // "function"
foo = 'hello';
```
</details>

## Scope (global, function, block)

**Beginner: Q1** - What are the different types of scope in JavaScript?

<details>
<summary>Answer</summary>

JavaScript has three main types of scope:

1. **Global Scope**: Variables accessible from anywhere in the program
2. **Function Scope**: Variables accessible only within the function where they're declared
3. **Block Scope**: Variables accessible only within the block `{}` where they're declared (ES6+)

```javascript
var globalVar = 'Global'; // Global scope

function myFunction() {
    var functionVar = 'Function'; // Function scope
    
    if (true) {
        var varInBlock = 'Var in block'; // Function scope (not block)
        let letInBlock = 'Let in block'; // Block scope
        const constInBlock = 'Const in block'; // Block scope
    }
    
    console.log(varInBlock); // Accessible
    // console.log(letInBlock); // ReferenceError
}
```
</details>

**Beginner: Q2** - What is the difference between global scope and function scope?

<details>
<summary>Answer</summary>

- **Global Scope**: Variables declared outside any function/block, accessible everywhere
- **Function Scope**: Variables declared inside a function, only accessible within that function

```javascript
var globalVar = 'I am global'; // Global scope

function test() {
    var functionVar = 'I am local'; // Function scope
    console.log(globalVar); // Can access global
    console.log(functionVar); // Can access local
}

test();
console.log(globalVar); // Can access global
// console.log(functionVar); // ReferenceError: not defined
```
</details>

**Beginner: Q3** - Do `var` declarations have block scope? What about `let` and `const`?

<details>
<summary>Answer</summary>

- **`var`**: No block scope, only function scope
- **`let` and `const`**: Yes, block-scoped

```javascript
if (true) {
    var varVariable = 'var';
    let letVariable = 'let';
    const constVariable = 'const';
}

console.log(varVariable); // 'var' - accessible
// console.log(letVariable); // ReferenceError
// console.log(constVariable); // ReferenceError

for (var i = 0; i < 3; i++) {
    // var i is function-scoped
}
console.log(i); // 3 - still accessible

for (let j = 0; j < 3; j++) {
    // let j is block-scoped
}
// console.log(j); // ReferenceError
```
</details>

**Intermediate: Q1** - What will be the output of this code and why?

```javascript
var i = 0;
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```

<details>
<summary>Answer</summary>

Output:
```
3
3
3
```

Explanation:
- `var` is function-scoped, so there's only one `i` variable
- The `for` loop completes before any `setTimeout` callback executes
- When callbacks run, `i` has already reached its final value of 3
- All three callbacks reference the same `i` variable

```javascript
// The loop is equivalent to:
var i = 0;
i = 0; // loop initialization
setTimeout(() => console.log(i), 100); // i will be 3
i = 1;
setTimeout(() => console.log(i), 100); // i will be 3  
i = 2;
setTimeout(() => console.log(i), 100); // i will be 3
i = 3; // loop ends
```
</details>

**Intermediate: Q2** - How would you fix the above code to print 0, 1, 2 instead?

<details>
<summary>Answer</summary>

Several solutions:

**Solution 1: Use `let` (block scope)**
```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 0, 1, 2
}
```

**Solution 2: Use IIFE to create closure**
```javascript
for (var i = 0; i < 3; i++) {
    (function(index) {
        setTimeout(() => console.log(index), 100);
    })(i);
}
```

**Solution 3: Use bind to pass the value**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(console.log.bind(null, i), 100);
}
```

**Solution 4: Pass parameter to setTimeout**
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout((index) => console.log(index), 100, i);
}
```
</details>

## Function declarations vs expressions

**Beginner: Q1** - What is the difference between a function declaration and a function expression?

<details>
<summary>Answer</summary>

| Function Declaration | Function Expression |
|---------------------|-------------------|
| Fully hoisted | Variable hoisted, function not |
| Can be called before definition | Cannot be called before definition |
| Creates named function | Can be anonymous or named |

```javascript
// Function Declaration
function declared() {
    return 'I am declared';
}

// Function Expression (anonymous)
var expressed = function() {
    return 'I am expressed';
};

// Function Expression (named)
var namedExpressed = function namedFunc() {
    return 'I am a named expression';
};

// Arrow Function Expression
var arrow = () => 'I am an arrow function';
```
</details>

**Beginner: Q2** - Which of these can be called before it's defined in the code?

<details>
<summary>Answer</summary>

Only **function declarations** can be called before they're defined:

```javascript
// This works - function declaration is hoisted
console.log(declared()); // "Hello from declaration"

function declared() {
    return "Hello from declaration";
}

// This doesn't work - only variable is hoisted, not the function
console.log(expressed()); // TypeError: expressed is not a function

var expressed = function() {
    return "Hello from expression";
};
```
</details>

**Beginner: Q3** - What is an anonymous function expression?

<details>
<summary>Answer</summary>

An anonymous function expression is a function without a name:

```javascript
// Anonymous function expression
var myFunc = function() {
    console.log('I have no name');
};

// Named function expression
var myFunc2 = function namedFunc() {
    console.log('I have a name: namedFunc');
};

// Anonymous arrow function
var myArrow = () => {
    console.log('I am an anonymous arrow function');
};

// Common use in callbacks
setTimeout(function() { // anonymous
    console.log('Timer done');
}, 1000);
```
</details>

**Intermediate: Q1** - What will be the output of this code?

```javascript
console.log(foo());
console.log(bar());

function foo() { return 'foo'; }
var bar = function() { return 'bar'; };
```

<details>
<summary>Answer</summary>

Output:
```
foo
TypeError: bar is not a function
```

Explanation:
- `foo()` works because function declarations are fully hoisted
- `bar()` fails because only the variable declaration is hoisted, not the function assignment

The code is interpreted as:
```javascript
var bar; // hoisted variable declaration
function foo() { return 'foo'; } // hoisted function declaration

console.log(foo()); // 'foo' - function is available
console.log(bar()); // TypeError - bar is undefined, not a function

bar = function() { return 'bar'; }; // assignment happens here
```
</details>

**Intermediate: Q2** - What is a named function expression and when would you use it?

<details>
<summary>Answer</summary>

A named function expression is a function expression with a name. The name is only available inside the function itself:

```javascript
var factorial = function fact(n) {
    return n <= 1 ? 1 : n * fact(n - 1); // 'fact' available inside
};

console.log(factorial(5)); // 25
// console.log(fact(5)); // ReferenceError: fact is not defined

// Use cases:
// 1. Recursion (cleaner than using the variable name)
// 2. Better stack traces for debugging
// 3. Self-referencing when the variable might be reassigned

var originalFunc = function myFunc() {
    console.log('Original function');
    return myFunc; // refers to the function itself
};

var newVar = originalFunc();
originalFunc = null; // variable reassigned
newVar(); // Still works because myFunc refers to the original function
```
</details>

## First-class functions

**Beginner: Q1** - What does it mean that functions are "first-class citizens" in JavaScript?

<details>
<summary>Answer</summary>

Functions are first-class citizens means they can be:
- Assigned to variables
- Passed as arguments to other functions
- Returned from functions
- Stored in data structures (arrays, objects)

```javascript
// Assign to variable
const greet = function() { return "Hello"; };

// Pass as argument
function execute(fn) { return fn(); }
execute(greet);

// Return from function
function createFunc() {
    return function() { return "Created"; };
}

// Store in array/object
const funcs = [greet, createFunc];
const obj = { method: greet };
```
</details>

**Beginner: Q2** - Can you assign a function to a variable? Can you pass a function as an argument?

<details>
<summary>Answer</summary>

Yes to both:

```javascript
// Assign function to variable
const myFunc = function(name) {
    return `Hello, ${name}`;
};

// Pass function as argument
function callFunction(fn, arg) {
    return fn(arg);
}

const result = callFunction(myFunc, "John"); // "Hello, John"

// Real-world example
const numbers = [1, 2, 3];
const doubled = numbers.map(function(x) { return x * 2; }); // [2, 4, 6]
```
</details>

**Beginner: Q3** - Can functions be returned from other functions?

<details>
<summary>Answer</summary>

Yes, functions can return other functions:

```javascript
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Arrow function version
const createAdder = (x) => (y) => x + y;
const add5 = createAdder(5);
console.log(add5(3)); // 8
```
</details>

**Intermediate: Q1** - Write a function that takes another function as a parameter and executes it twice.

<details>
<summary>Answer</summary>

```javascript
function executeTwice(fn, arg) {
    fn(arg);
    fn(arg);
}

// Example usage
function sayHello(name) {
    console.log(`Hello, ${name}!`);
}

executeTwice(sayHello, "John");
// Output:
// Hello, John!
// Hello, John!

// More flexible version
function repeatFunction(fn, times, ...args) {
    for (let i = 0; i < times; i++) {
        fn(...args);
    }
}

repeatFunction(console.log, 3, "Repeated message");
```
</details>

**Intermediate: Q2** - How would you store functions in an array and execute them?

<details>
<summary>Answer</summary>

```javascript
// Store functions in array
const operations = [
    function(x) { return x + 1; },
    function(x) { return x * 2; },
    function(x) { return x - 1; }
];

// Execute all functions
const input = 5;
const results = operations.map(fn => fn(input));
console.log(results); // [6, 10, 4]

// Execute functions sequentially (pipeline)
function pipeline(value, functions) {
    return functions.reduce((acc, fn) => fn(acc), value);
}

const result = pipeline(5, operations); // ((5 + 1) * 2) - 1 = 11
console.log(result); // 11
```
</details>

## Higher-order functions

**Beginner: Q1** - What is a higher-order function? Give an example.

<details>
<summary>Answer</summary>

A higher-order function is a function that either:
- Takes one or more functions as arguments, OR
- Returns a function as its result

```javascript
// Takes function as argument
function applyOperation(x, y, operation) {
    return operation(x, y);
}

const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

console.log(applyOperation(5, 3, add)); // 8
console.log(applyOperation(5, 3, multiply)); // 15

// Returns function
function createLogger(prefix) {
    return function(message) {
        console.log(`[${prefix}] ${message}`);
    };
}

const errorLogger = createLogger('ERROR');
errorLogger('Something went wrong'); // [ERROR] Something went wrong
```
</details>

**Beginner: Q2** - Name three built-in higher-order functions in JavaScript arrays.

<details>
<summary>Answer</summary>

1. **`map()`** - Transforms each element
2. **`filter()`** - Filters elements based on condition  
3. **`reduce()`** - Reduces array to single value

```javascript
const numbers = [1, 2, 3, 4, 5];

// map - transforms each element
const doubled = numbers.map(x => x * 2); // [2, 4, 6, 8, 10]

// filter - keeps elements that pass test
const evens = numbers.filter(x => x % 2 === 0); // [2, 4]

// reduce - combines all elements
const sum = numbers.reduce((acc, x) => acc + x, 0); // 15

// Other common ones: forEach, find, some, every, sort
```
</details>

**Beginner: Q3** - What does the `map()` function do?

<details>
<summary>Answer</summary>

`map()` creates a new array by calling a function on every element of the original array:

```javascript
const numbers = [1, 2, 3, 4];

// Transform numbers
const squared = numbers.map(x => x * x); // [1, 4, 9, 16]

// Transform objects
const users = [
    { name: 'John', age: 25 },
    { name: 'Jane', age: 30 }
];

const names = users.map(user => user.name); // ['John', 'Jane']
const ages = users.map(user => user.age); // [25, 30]

// Original array unchanged
console.log(numbers); // [1, 2, 3, 4]
```
</details>

**Intermediate: Q1** - Implement your own version of the `filter()` function.

<details>
<summary>Answer</summary>

```javascript
function myFilter(array, callback) {
    const result = [];
    for (let i = 0; i < array.length; i++) {
        if (callback(array[i], i, array)) {
            result.push(array[i]);
        }
    }
    return result;
}

// Usage example
const numbers = [1, 2, 3, 4, 5, 6];
const evens = myFilter(numbers, x => x % 2 === 0);
console.log(evens); // [2, 4, 6]

// As Array prototype method
Array.prototype.myFilter = function(callback) {
    const result = [];
    for (let i = 0; i < this.length; i++) {
        if (callback(this[i], i, this)) {
            result.push(this[i]);
        }
    }
    return result;
};
```
</details>

**Intermediate: Q2** - What is the difference between `map()`, `forEach()`, and `filter()`?

<details>
<summary>Answer</summary>

| Method | Returns | Purpose | Mutates Original |
|--------|---------|---------|------------------|
| `map()` | New array | Transform elements | No |
| `filter()` | New array | Filter elements | No |
| `forEach()` | `undefined` | Side effects only | No* |

```javascript
const numbers = [1, 2, 3, 4];

// map - transforms and returns new array
const doubled = numbers.map(x => x * 2); // [2, 4, 6, 8]

// filter - filters and returns new array  
const evens = numbers.filter(x => x % 2 === 0); // [2, 4]

// forEach - no return value, used for side effects
numbers.forEach(x => console.log(x)); // prints each number
const result = numbers.forEach(x => x * 2); // undefined

// *forEach doesn't mutate, but callback might
const objects = [{a: 1}, {a: 2}];
objects.forEach(obj => obj.a *= 2); // mutates objects
```
</details>

## Closures

**Beginner: Q1** - What is a closure in JavaScript?

<details>
<summary>Answer</summary>

A closure is a function that has access to variables from its outer (enclosing) scope even after the outer function has finished executing.

```javascript
function outerFunction(x) {
    // Outer scope variable
    const outerVariable = x;
    
    // Inner function has access to outerVariable
    function innerFunction(y) {
        return outerVariable + y; // accessing outer scope
    }
    
    return innerFunction;
}

const closure = outerFunction(10);
console.log(closure(5)); // 15 - still has access to outerVariable
```
</details>

**Beginner: Q2** - Give a simple example of a closure.

<details>
<summary>Answer</summary>

```javascript
function createCounter() {
    let count = 0;
    
    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

// Each counter has its own closure
const counter2 = createCounter();
console.log(counter2()); // 1 (independent counter)
```
</details>

**Beginner: Q3** - Why do closures have access to outer function variables even after the outer function returns?

<details>
<summary>Answer</summary>

Because JavaScript maintains a reference to the outer function's variables in the closure's lexical environment. The variables are not destroyed when the outer function returns if they're still referenced by the inner function.

```javascript
function createMessage(name) {
    const greeting = `Hello, ${name}!`;
    
    // Even after createMessage returns, 
    // greeting is kept alive in closure
    return function() {
        return greeting;
    };
}

const getMessage = createMessage("John");
// createMessage has finished executing, but...
console.log(getMessage()); // "Hello, John!" - greeting still accessible
```
</details>

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

<details>
<summary>Answer</summary>

Output:
```
10
11
```

Explanation:
- First call: `x` is 10, then incremented to 11
- Second call: `x` is now 11 (persisted in closure), then incremented to 12
- The variable `x` is shared between calls and maintains its state

```javascript
// The closure maintains reference to the same x variable
var fn = outer(); // x = 10 in closure
fn(); // prints 10, x becomes 11
fn(); // prints 11, x becomes 12
```
</details>

**Intermediate: Q2** - How can closures lead to memory leaks? How would you prevent them?

<details>
<summary>Answer</summary>

Closures can cause memory leaks when they hold references to large objects or DOM elements that are no longer needed:

```javascript
// Memory leak example
function attachListener() {
    const largeData = new Array(1000000).fill('data');
    const element = document.getElementById('button');
    
    element.addEventListener('click', function() {
        // Closure keeps largeData in memory even if not used
        console.log('Clicked');
    });
}

// Prevention strategies:
// 1. Remove event listeners
function attachListenerSafely() {
    const element = document.getElementById('button');
    
    function clickHandler() {
        console.log('Clicked');
        // Clean up when done
        element.removeEventListener('click', clickHandler);
    }
    
    element.addEventListener('click', clickHandler);
}

// 2. Nullify references
function createClosureSafely() {
    let largeData = new Array(1000000).fill('data');
    
    return function() {
        // Use largeData...
        largeData = null; // Release reference when done
    };
}
```
</details>

## IIFE (Immediately Invoked Function Expressions)

**Beginner: Q1** - What is an IIFE? Write the syntax for one.

<details>
<summary>Answer</summary>

IIFE (Immediately Invoked Function Expression) is a function that executes immediately after being defined.

```javascript
// Basic IIFE syntax
(function() {
    console.log("I execute immediately!");
})();

// Alternative syntax
(function() {
    console.log("Alternative syntax");
}());

// With parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})("John");

// Arrow function IIFE
(() => {
    console.log("Arrow IIFE");
})();

// IIFE with return value
const result = (function(a, b) {
    return a + b;
})(5, 3); // result = 8
```
</details>

**Beginner: Q2** - Why would you use an IIFE?

<details>
<summary>Answer</summary>

Main reasons to use IIFE:

1. **Avoid global namespace pollution**
2. **Create private scope**
3. **Execute initialization code immediately**
4. **Module pattern implementation**

```javascript
// Avoid global pollution
(function() {
    var privateVar = "I'm private";
    // Code here doesn't pollute global scope
})();

// Module pattern
const myModule = (function() {
    let privateCounter = 0;
    
    return {
        increment: function() { privateCounter++; },
        getCount: function() { return privateCounter; }
    };
})();

myModule.increment();
console.log(myModule.getCount()); // 1
// privateCounter is not accessible directly
```
</details>

**Beginner: Q3** - Can you pass parameters to an IIFE?

<details>
<summary>Answer</summary>

Yes, you can pass parameters to an IIFE:

```javascript
// Pass parameters
(function(name, age) {
    console.log(`Name: ${name}, Age: ${age}`);
})("John", 25);

// Common pattern: pass global objects
(function($, window, document) {
    // Use $ instead of jQuery, safer in case $ is overwritten
    $('body').addClass('loaded');
})(jQuery, window, document);

// Pass configuration object
(function(config) {
    console.log(`Environment: ${config.env}`);
    console.log(`Debug: ${config.debug}`);
})({
    env: 'development',
    debug: true
});
```
</details>

**Intermediate: Q1** - How were IIFEs used to create modules before ES6 modules?

<details>
<summary>Answer</summary>

IIFEs created the module pattern by returning public methods while keeping private variables:

```javascript
// Module pattern with IIFE
const Calculator = (function() {
    // Private variables and functions
    let result = 0;
    
    function log(operation, value) {
        console.log(`${operation}: ${value}`);
    }
    
    // Public API
    return {
        add: function(x) {
            result += x;
            log('Add', x);
            return this;
        },
        subtract: function(x) {
            result -= x;
            log('Subtract', x);
            return this;
        },
        getResult: function() {
            return result;
        },
        reset: function() {
            result = 0;
            return this;
        }
    };
})();

// Usage
Calculator.add(5).subtract(2).add(3);
console.log(Calculator.getResult()); // 6
// result and log are private, not accessible
```
</details>

**Intermediate: Q2** - What's the difference between these two IIFE syntaxes?

```javascript
(function() { })();
(function() { }());
```

<details>
<summary>Answer</summary>

Both syntaxes work identically - it's just a matter of preference:

```javascript
// Style 1: Function expression in parentheses, then invoke
(function() { 
    console.log("Style 1"); 
})();

// Style 2: Entire invocation in parentheses
(function() { 
    console.log("Style 2"); 
}());

// Both compile to the same thing and execute immediately
// Douglas Crockford prefers style 1
// Some style guides prefer style 2

// Other valid IIFE patterns:
!function() { console.log("Using !"); }();
+function() { console.log("Using +"); }();
void function() { console.log("Using void"); }();
```

The key is that the function must be treated as an expression, not a declaration.
</details>

## Recursion

**Beginner: Q1** - What is recursion? Write a simple recursive function to calculate factorial.

<details>
<summary>Answer</summary>

Recursion is when a function calls itself to solve a problem by breaking it into smaller subproblems.

```javascript
function factorial(n) {
    // Base case
    if (n <= 1) {
        return 1;
    }
    // Recursive case
    return n * factorial(n - 1);
}

console.log(factorial(5)); // 120 (5 × 4 × 3 × 2 × 1)

// Step-by-step execution:
// factorial(5) = 5 * factorial(4)
// factorial(4) = 4 * factorial(3)
// factorial(3) = 3 * factorial(2)
// factorial(2) = 2 * factorial(1)
// factorial(1) = 1 (base case)
// Working back: 1, then 2*1=2, then 3*2=6, then 4*6=24, then 5*24=120
```
</details>

**Beginner: Q2** - What are the two essential parts of a recursive function?

<details>
<summary>Answer</summary>

Every recursive function must have:

1. **Base case**: A condition that stops the recursion
2. **Recursive case**: The function calls itself with modified parameters

```javascript
function countdown(n) {
    // Base case - stops recursion
    if (n <= 0) {
        console.log("Done!");
        return;
    }
    
    console.log(n);
    // Recursive case - calls itself with n-1
    countdown(n - 1);
}

countdown(3); // Output: 3, 2, 1, Done!

// Without base case - infinite recursion (stack overflow)
function infiniteRecursion(n) {
    console.log(n);
    infiniteRecursion(n + 1); // No base case!
}
```
</details>

**Beginner: Q3** - What happens if you forget the base case in recursion?

<details>
<summary>Answer</summary>

Without a base case, you get **infinite recursion** leading to a **stack overflow error**.

```javascript
// Missing base case - BAD!
function badRecursion(n) {
    console.log(n);
    return badRecursion(n - 1); // Never stops!
}

// This will throw: RangeError: Maximum call stack size exceeded
// badRecursion(5);

// Correct version with base case
function goodRecursion(n) {
    if (n <= 0) return; // Base case
    console.log(n);
    goodRecursion(n - 1);
}

// JavaScript call stack limit is usually around 10,000-15,000 calls
function testStackLimit() {
    let count = 0;
    function recurse() {
        count++;
        recurse();
    }
    
    try {
        recurse();
    } catch (e) {
        console.log(`Stack overflow at ${count} calls`);
    }
}
```
</details>

**Intermediate: Q1** - Write a recursive function to flatten a nested array.

<details>
<summary>Answer</summary>

```javascript
function flattenArray(arr) {
    let result = [];
    
    for (let element of arr) {
        if (Array.isArray(element)) {
            // Recursive case: element is an array
            result.push(...flattenArray(element));
        } else {
            // Base case: element is not an array
            result.push(element);
        }
    }
    
    return result;
}

const nested = [1, [2, 3], [4, [5, 6]], 7];
console.log(flattenArray(nested)); // [1, 2, 3, 4, 5, 6, 7]

// One-liner using reduce
function flattenRecursive(arr) {
    return arr.reduce((flat, item) => 
        flat.concat(Array.isArray(item) ? flattenRecursive(item) : item), []
    );
}

// Handle deeply nested arrays
const deepNested = [1, [2, [3, [4, [5]]]]];
console.log(flattenArray(deepNested)); // [1, 2, 3, 4, 5]

// ES2019 has built-in flat() method
console.log(deepNested.flat(Infinity)); // [1, 2, 3, 4, 5]
```
</details>

**Intermediate: Q2** - What is tail recursion and how does it help with performance?

<details>
<summary>Answer</summary>

**Tail recursion** occurs when the recursive call is the last operation in the function (in tail position).

```javascript
// Regular recursion - NOT tail recursive
function factorialRegular(n) {
    if (n <= 1) return 1;
    return n * factorialRegular(n - 1); // Operation after recursive call
}

// Tail recursive version
function factorialTail(n, accumulator = 1) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator); // Last operation
}

// Comparison
console.log(factorialRegular(5)); // 120
console.log(factorialTail(5));    // 120

// Tail recursion benefits:
// 1. Can be optimized to use constant stack space
// 2. Converted to a loop by compiler
// 3. Prevents stack overflow for large inputs

// Note: JavaScript engines don't widely support tail call optimization
// Convert to iteration for better performance:
function factorialIterative(n) {
    let result = 1;
    for (let i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```
</details>

## Currying

**Beginner: Q1** - What is currying in JavaScript?

<details>
<summary>Answer</summary>

Currying transforms a function that takes multiple arguments into a sequence of functions, each taking a single argument.

```javascript
// Regular function
function add(a, b, c) {
    return a + b + c;
}

console.log(add(1, 2, 3)); // 6

// Curried version
function addCurried(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

console.log(addCurried(1)(2)(3)); // 6

// Arrow function currying
const addArrow = a => b => c => a + b + c;
console.log(addArrow(1)(2)(3)); // 6

// Partial application
const addOne = addCurried(1);
const addOneTwo = addOne(2);
console.log(addOneTwo(3)); // 6
```
</details>

**Beginner: Q2** - Convert this function to a curried version: `function add(a, b, c) { return a + b + c; }`

<details>
<summary>Answer</summary>

```javascript
// Original function
function add(a, b, c) {
    return a + b + c;
}

// Method 1: Manual currying
function addCurried(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

// Method 2: Arrow function syntax
const addCurriedArrow = a => b => c => a + b + c;

// Method 3: Using bind
function addWithBind(a, b, c) {
    return a + b + c;
}

const curriedWithBind = a => addWithBind.bind(null, a);

// Usage examples
console.log(addCurried(1)(2)(3)); // 6
console.log(addCurriedArrow(2)(3)(4)); // 9

// Partial application benefits
const add5 = addCurried(5);
const add5And10 = add5(10);
console.log(add5And10(3)); // 18
```
</details>

**Beginner: Q3** - What is the advantage of currying?

<details>
<summary>Answer</summary>

Key advantages of currying:

1. **Reusability**: Create specialized functions
2. **Partial application**: Fix some arguments
3. **Function composition**: Chain functions easily
4. **Configuration**: Create configured functions

```javascript
// 1. Reusability
const multiply = a => b => a * b;
const double = multiply(2);
const triple = multiply(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 2. Event handling
const addEventListener = element => event => handler => {
    element.addEventListener(event, handler);
};

const button = document.querySelector('button');
const addClickListener = addEventListener(button)('click');
addClickListener(() => console.log('Clicked!'));

// 3. Function composition
const add = a => b => a + b;
const subtract = a => b => b - a;

const addThenSubtract = x => y => z => subtract(5)(add(x)(y));

// 4. Configuration functions
const apiCall = baseUrl => endpoint => params => {
    return fetch(`${baseUrl}/${endpoint}`, params);
};

const myApi = apiCall('https://api.example.com');
const getUsers = myApi('users');
const getPosts = myApi('posts');
```
</details>

**Intermediate: Q1** - Implement a generic curry function that can curry any function.

<details>
<summary>Answer</summary>

```javascript
// Generic curry function
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            // All arguments provided, call original function
            return fn.apply(this, args);
        } else {
            // Not enough arguments, return function waiting for more
            return function(...nextArgs) {
                return curried.apply(this, args.concat(nextArgs));
            };
        }
    };
}

// Test with different functions
function add(a, b, c) {
    return a + b + c;
}

function multiply(a, b, c, d) {
    return a * b * c * d;
}

// Curry the functions
const curriedAdd = curry(add);
const curriedMultiply = curry(multiply);

// Usage examples
console.log(curriedAdd(1)(2)(3)); // 6
console.log(curriedAdd(1, 2)(3)); // 6
console.log(curriedAdd(1)(2, 3)); // 6
console.log(curriedAdd(1, 2, 3)); // 6

console.log(curriedMultiply(1)(2)(3)(4)); // 24
console.log(curriedMultiply(1, 2)(3, 4)); // 24

// Advanced curry with placeholder support
function advancedCurry(fn) {
    const placeholder = Symbol('placeholder');
    
    function curried(...args) {
        if (args.length >= fn.length && !args.includes(placeholder)) {
            return fn.apply(this, args);
        }
        
        return function(...nextArgs) {
            const newArgs = args.map(arg => 
                arg === placeholder && nextArgs.length ? nextArgs.shift() : arg
            ).concat(nextArgs);
            
            return curried.apply(this, newArgs);
        };
    }
    
    curried.placeholder = placeholder;
    return curried;
}
```
</details>

**Intermediate: Q2** - What is partial application and how is it different from currying?

<details>
<summary>Answer</summary>

**Partial Application**: Fixing some arguments of a function to create a new function.
**Currying**: Transforming a function to take arguments one at a time.

```javascript
// Original function
function multiply(a, b, c) {
    return a * b * c;
}

// PARTIAL APPLICATION
// Using bind for partial application
const multiplyByTwo = multiply.bind(null, 2);
console.log(multiplyByTwo(3, 4)); // 24 (2 * 3 * 4)

// Custom partial function
function partial(fn, ...partialArgs) {
    return function(...remainingArgs) {
        return fn(...partialArgs, ...remainingArgs);
    };
}

const partialMultiply = partial(multiply, 2, 3);
console.log(partialMultiply(4)); // 24 (2 * 3 * 4)

// CURRYING
function curriedMultiply(a) {
    return function(b) {
        return function(c) {
            return a * b * c;
        };
    };
}

console.log(curriedMultiply(2)(3)(4)); // 24

// Key differences:
// 1. Partial application can fix multiple arguments at once
// 2. Currying always returns unary functions (one argument)
// 3. Partial application creates functions that may take multiple arguments
// 4. Currying transforms function structure, partial application doesn't

// Practical example
const log = (level, module, message) => {
    console.log(`[${level}] ${module}: ${message}`);
};

// Partial application - fix level and module
const errorLogger = partial(log, 'ERROR', 'UserService');
errorLogger('User not found'); // [ERROR] UserService: User not found

// Currying - one argument at a time
const curriedLog = level => module => message => {
    console.log(`[${level}] ${module}: ${message}`);
};

const errorLog = curriedLog('ERROR');
const userErrorLog = errorLog('UserService');
userErrorLog('User not found');
```
</details>

## Pure vs impure functions

**Beginner: Q1** - What is a pure function? Give an example.

<details>
<summary>Answer</summary>

A pure function:
1. Always returns the same output for the same input
2. Has no side effects (doesn't modify external state)

```javascript
// Pure function
function add(a, b) {
    return a + b;
}

console.log(add(2, 3)); // Always returns 5
console.log(add(2, 3)); // Always returns 5

// Pure function - array sum
function sum(numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

const nums = [1, 2, 3];
console.log(sum(nums)); // Always returns 6
// Original array unchanged
```
</details>

**Beginner: Q2** - What is an impure function? Give an example.

<details>
<summary>Answer</summary>

An impure function:
1. Can return different outputs for the same input
2. Has side effects (modifies external state)

```javascript
let count = 0;

// Impure - modifies external variable
function increment() {
    count++; // Side effect
    return count;
}

console.log(increment()); // 1
console.log(increment()); // 2 (different output)

// Impure - depends on external state
function getCurrentTime() {
    return new Date().getTime(); // Different each time
}

// Impure - modifies input
function addToArray(arr, item) {
    arr.push(item); // Mutates original array
    return arr;
}
```
</details>

**Beginner: Q3** - What are the benefits of using pure functions?

<details>
<summary>Answer</summary>

Benefits of pure functions:

1. **Predictable**: Same input = same output
2. **Testable**: Easy to unit test
3. **Cacheable**: Can memoize results
4. **Debuggable**: No hidden dependencies
5. **Parallelizable**: Safe for concurrent execution

```javascript
// Easy to test
function multiply(a, b) {
    return a * b;
}
// Test: expect(multiply(2, 3)).toBe(6)

// Cacheable/Memoizable
const memoizedFactorial = (() => {
    const cache = {};
    return function factorial(n) {
        if (n in cache) return cache[n];
        if (n <= 1) return 1;
        cache[n] = n * factorial(n - 1);
        return cache[n];
    };
})();

// No unexpected behavior
function calculateTax(price, rate) {
    return price * rate; // Always predictable
}
```
</details>

**Intermediate: Q1** - Is this function pure or impure? Explain why.
```javascript
let counter = 0;
function increment() {
    return ++counter;
}
```

<details>
<summary>Answer</summary>

This function is **IMPURE** for two reasons:

1. **Side effect**: Modifies external variable `counter`
2. **Non-deterministic**: Returns different values on each call

```javascript
let counter = 0;
function increment() {
    return ++counter; // Modifies external state
}

console.log(increment()); // 1
console.log(increment()); // 2 - different output!

// Pure version:
function pureIncrement(value) {
    return value + 1; // No side effects
}

console.log(pureIncrement(5)); // Always 6
console.log(pureIncrement(5)); // Always 6

// If you need state, return new state
function incrementCounter(counter) {
    return { count: counter.count + 1 };
}
```
</details>

**Intermediate: Q2** - How do pure functions relate to functional programming concepts like immutability?

<details>
<summary>Answer</summary>

Pure functions and immutability work together in functional programming:

```javascript
// Immutable operations with pure functions
const numbers = [1, 2, 3];

// Pure - returns new array, doesn't mutate
function addNumber(arr, num) {
    return [...arr, num]; // Creates new array
}

// Pure - transforms without mutation
function doubleNumbers(arr) {
    return arr.map(num => num * 2); // New array
}

console.log(addNumber(numbers, 4)); // [1, 2, 3, 4]
console.log(numbers); // [1, 2, 3] - unchanged

// Object immutability
function updateUser(user, updates) {
    return { ...user, ...updates }; // New object
}

const user = { name: 'John', age: 30 };
const updated = updateUser(user, { age: 31 });
console.log(user); // { name: 'John', age: 30 } - unchanged

// Functional composition with pure functions
const pipe = (...fns) => value => fns.reduce((acc, fn) => fn(acc), value);

const process = pipe(
    arr => arr.filter(x => x > 0),
    arr => arr.map(x => x * 2),
    arr => arr.reduce((sum, x) => sum + x, 0)
);

console.log(process([-1, 2, 3, -4])); // 10
```
</details>

## Object creation ({}, Object.create, classes)

**Beginner: Q1** - What are the different ways to create objects in JavaScript?

<details>
<summary>Answer</summary>

JavaScript offers several ways to create objects:

```javascript
// 1. Object literal
const obj1 = { name: 'John', age: 30 };

// 2. Object constructor
const obj2 = new Object();
obj2.name = 'John';

// 3. Constructor function
function Person(name, age) {
    this.name = name;
    this.age = age;
}
const obj3 = new Person('John', 30);

// 4. Object.create()
const obj4 = Object.create({ name: 'John' });

// 5. ES6 Classes
class PersonClass {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
const obj5 = new PersonClass('John', 30);

// 6. Factory function
function createPerson(name, age) {
    return { name, age };
}
const obj6 = createPerson('John', 30);
```
</details>

**Beginner: Q2** - What is the difference between object literal syntax and using `new Object()`?

<details>
<summary>Answer</summary>

Both create objects, but object literal is preferred:

```javascript
// Object literal - preferred
const obj1 = {
    name: 'John',
    age: 30,
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};

// Object constructor - verbose
const obj2 = new Object();
obj2.name = 'John';
obj2.age = 30;
obj2.greet = function() {
    return `Hello, I'm ${this.name}`;
};

// Differences:
// 1. Syntax: Literal is more concise
// 2. Performance: Literal is slightly faster
// 3. Readability: Literal is clearer
// 4. Functionality: Both create same result

console.log(obj1.greet()); // "Hello, I'm John"
console.log(obj2.greet()); // "Hello, I'm John"
console.log(obj1.constructor === Object); // true
console.log(obj2.constructor === Object); // true
```
</details>

**Beginner: Q3** - How do you create an object using a constructor function?

<details>
<summary>Answer</summary>

Constructor functions create objects with `new` keyword:

```javascript
// Constructor function (capitalized by convention)
function Person(name, age, city) {
    this.name = name;
    this.age = age;
    this.city = city;
    
    this.greet = function() {
        return `Hi, I'm ${this.name} from ${this.city}`;
    };
}

// Create instances
const person1 = new Person('John', 30, 'NYC');
const person2 = new Person('Jane', 25, 'LA');

console.log(person1.greet()); // "Hi, I'm John from NYC"
console.log(person1 instanceof Person); // true

// What happens with 'new':
// 1. Creates empty object {}
// 2. Sets this = new object
// 3. Sets object.__proto__ = Person.prototype
// 4. Returns the object

// Without 'new' (avoid this)
const badPerson = Person('Bob', 40, 'Chicago'); // undefined
console.log(window.name); // 'Bob' (global pollution!)
```
</details>

**Intermediate: Q1** - What is `Object.create()` and how is it different from other object creation methods?

<details>
<summary>Answer</summary>

`Object.create()` creates objects with specific prototype:

```javascript
// Object.create() with prototype
const personPrototype = {
    greet() {
        return `Hello, I'm ${this.name}`;
    },
    getAge() {
        return this.age;
    }
};

const person = Object.create(personPrototype);
person.name = 'John';
person.age = 30;

console.log(person.greet()); // "Hello, I'm John"
console.log(person.__proto__ === personPrototype); // true

// With property descriptors
const personWithProps = Object.create(personPrototype, {
    name: {
        value: 'Jane',
        writable: true,
        enumerable: true
    },
    age: {
        value: 25,
        writable: false // Read-only
    }
});

// Differences from other methods:
// 1. Direct prototype control
// 2. No constructor function needed
// 3. Can create objects with null prototype

const nullProtoObj = Object.create(null);
console.log(nullProtoObj.toString); // undefined (no inherited methods)

// Comparison
const literal = { name: 'John' }; // prototype: Object.prototype
const created = Object.create({}); // prototype: {} (which has Object.prototype)
const nullCreated = Object.create(null); // prototype: null
```
</details>

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
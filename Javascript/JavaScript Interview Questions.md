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

<details>
<summary>Answer</summary>

ES6 classes are syntactic sugar over constructor functions:

```javascript
// Constructor Function
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    return `Hi, I'm ${this.name}`;
};

// ES6 Class
class PersonClass {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hi, I'm ${this.name}`;
    }
}

// Both create similar objects
const person1 = new Person('John', 30);
const person2 = new PersonClass('Jane', 25);

// Key differences:
// 1. Hoisting: Functions are hoisted, classes are not
console.log(new Person('Test')); // Works
// console.log(new PersonClass('Test')); // ReferenceError

// 2. Strict mode: Classes automatically use strict mode
// 3. Enumerable: Class methods are non-enumerable
// 4. Constructor calls: Classes must be called with 'new'

// PersonClass(); // TypeError: Class constructor cannot be invoked without 'new'
```
</details>

## Prototype chain & inheritance

**Beginner: Q1** - What is the prototype chain in JavaScript?

<details>
<summary>Answer</summary>

The prototype chain is JavaScript's inheritance mechanism where objects can access properties/methods from their prototype objects.

```javascript
// Every object has a prototype
const obj = { name: 'John' };
console.log(obj.__proto__ === Object.prototype); // true

// Prototype chain lookup
console.log(obj.toString()); // Inherited from Object.prototype

// Constructor function prototype
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, ${this.name}`;
};

const person = new Person('John');

// Chain: person -> Person.prototype -> Object.prototype -> null
console.log(person.greet()); // Found on Person.prototype
console.log(person.toString()); // Found on Object.prototype
console.log(person.nonExistent); // undefined (not found anywhere)
```
</details>

**Beginner: Q2** - How do you add a method to all instances of a constructor function?

<details>
<summary>Answer</summary>

Add methods to the constructor's `prototype` property:

```javascript
function Car(brand, model) {
    this.brand = brand;
    this.model = model;
}

// Add method to prototype (shared by all instances)
Car.prototype.getInfo = function() {
    return `${this.brand} ${this.model}`;
};

Car.prototype.start = function() {
    return `${this.brand} is starting...`;
};

const car1 = new Car('Toyota', 'Camry');
const car2 = new Car('Honda', 'Civic');

console.log(car1.getInfo()); // "Toyota Camry"
console.log(car2.start()); // "Honda is starting..."

// Method exists on prototype, not on instance
console.log(car1.hasOwnProperty('getInfo')); // false
console.log(Car.prototype.hasOwnProperty('getInfo')); // true

// More efficient than adding to each instance
function BadCar(brand) {
    this.brand = brand;
    this.getInfo = function() { // Creates new function for each instance
        return this.brand;
    };
}
```
</details>

**Beginner: Q3** - What happens when you try to access a property that doesn't exist on an object?

<details>
<summary>Answer</summary>

JavaScript searches up the prototype chain, returns `undefined` if not found:

```javascript
const person = { name: 'John' };

// Property lookup process:
// 1. Check person object
// 2. Check person.__proto__ (Object.prototype)
// 3. Check Object.prototype.__proto__ (null)
// 4. Return undefined if not found

console.log(person.name); // 'John' (found on object)
console.log(person.toString); // function (found on Object.prototype)
console.log(person.nonExistent); // undefined (not found anywhere)

// Prototype chain example
function Animal(name) {
    this.name = name;
}

Animal.prototype.species = 'Unknown';

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.sound = 'Woof';

const dog = new Dog('Buddy', 'Golden Retriever');

console.log(dog.name); // 'Buddy' (own property)
console.log(dog.breed); // 'Golden Retriever' (own property)
console.log(dog.sound); // 'Woof' (Dog.prototype)
console.log(dog.species); // 'Unknown' (Animal.prototype)
console.log(dog.toString); // function (Object.prototype)
console.log(dog.flying); // undefined (not found)
```
</details>

**Intermediate: Q1** - Explain prototypal inheritance with an example.

<details>
<summary>Answer</summary>

Prototypal inheritance allows objects to inherit properties from other objects:

```javascript
// Parent constructor
function Animal(name) {
    this.name = name;
}

Animal.prototype.eat = function() {
    return `${this.name} is eating`;
};

Animal.prototype.sleep = function() {
    return `${this.name} is sleeping`;
};

// Child constructor
function Dog(name, breed) {
    Animal.call(this, name); // Call parent constructor
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // Fix constructor reference

// Add child-specific methods
Dog.prototype.bark = function() {
    return `${this.name} says Woof!`;
};

// Override parent method
Dog.prototype.eat = function() {
    return `${this.name} the ${this.breed} is eating dog food`;
};

const dog = new Dog('Buddy', 'Labrador');

console.log(dog.bark()); // "Buddy says Woof!" (own method)
console.log(dog.eat()); // "Buddy the Labrador is eating dog food" (overridden)
console.log(dog.sleep()); // "Buddy is sleeping" (inherited)

// Prototype chain: dog -> Dog.prototype -> Animal.prototype -> Object.prototype -> null
console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true
console.log(dog instanceof Object); // true
```
</details>

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

<details>
<summary>Answer</summary>

Output:
```
"Dog barks"
"Animal speaks"
```

Explanation:

```javascript
function Animal() {}
Animal.prototype.speak = function() { return 'Animal speaks'; };

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.speak = function() { return 'Dog barks'; };

const dog = new Dog();

// First call: dog.speak() finds method on Dog.prototype
console.log(dog.speak()); // "Dog barks"

// Delete removes the method from Dog.prototype
delete Dog.prototype.speak;

// Second call: dog.speak() doesn't find method on Dog.prototype,
// so it looks up the prototype chain and finds it on Animal.prototype
console.log(dog.speak()); // "Animal speaks"

// Prototype chain lookup:
// dog -> Dog.prototype (no speak method after delete) -> Animal.prototype (has speak method)
```
</details>

## __proto__ vs prototype

**Beginner: Q1** - What is the difference between `__proto__` and `prototype`?

<details>
<summary>Answer</summary>

- `prototype`: Property of constructor functions, defines what instances will inherit
- `__proto__`: Property of objects, points to the object it inherits from

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, ${this.name}`;
};

const john = new Person('John');

// prototype: Only on constructor functions
console.log(Person.prototype); // { greet: function, constructor: Person }
console.log(john.prototype); // undefined (instances don't have prototype)

// __proto__: On all objects, points to prototype
console.log(john.__proto__ === Person.prototype); // true
console.log(Person.__proto__ === Function.prototype); // true

// Relationship
// john.__proto__ -> Person.prototype -> Object.prototype -> null
```
</details>

**Beginner: Q2** - When you create an object with `new`, what does `__proto__` point to?

<details>
<summary>Answer</summary>

When using `new`, the object's `__proto__` points to the constructor's `prototype`:

```javascript
function Car(brand) {
    this.brand = brand;
}

Car.prototype.start = function() {
    return `${this.brand} is starting`;
};

const myCar = new Car('Toyota');

// __proto__ points to constructor's prototype
console.log(myCar.__proto__ === Car.prototype); // true
console.log(myCar.__proto__.start === Car.prototype.start); // true

// This is how inheritance works
console.log(myCar.start()); // "Toyota is starting"

// What happens with new:
// 1. Create empty object: {}
// 2. Set __proto__: newObj.__proto__ = Car.prototype
// 3. Call constructor: Car.call(newObj, 'Toyota')
// 4. Return the object

// Different constructors, different __proto__
function Bike(brand) { this.brand = brand; }
const myBike = new Bike('Honda');

console.log(myCar.__proto__ === Car.prototype); // true
console.log(myBike.__proto__ === Bike.prototype); // true
console.log(myCar.__proto__ === myBike.__proto__); // false
```
</details>

**Beginner: Q3** - Is `__proto__` a standard property or should you avoid using it?

<details>
<summary>Answer</summary>

`__proto__` is deprecated. Use `Object.getPrototypeOf()` and `Object.setPrototypeOf()` instead:

```javascript
function Person(name) {
    this.name = name;
}

const john = new Person('John');

// ❌ Avoid: __proto__ (deprecated)
console.log(john.__proto__);
john.__proto__ = Person.prototype;

// ✅ Use: Object.getPrototypeOf()
console.log(Object.getPrototypeOf(john) === Person.prototype); // true

// ✅ Use: Object.setPrototypeOf()
const newProto = { species: 'Human' };
Object.setPrototypeOf(john, newProto);
console.log(john.species); // 'Human'

// ✅ Use: Object.create() for creating with specific prototype
const jane = Object.create(Person.prototype);
jane.name = 'Jane';

// ✅ Use: instanceof for prototype checking
console.log(john instanceof Person); // true

// Why avoid __proto__:
// 1. Not standardized in older versions
// 2. Performance implications
// 3. Better alternatives available
```
</details>

**Intermediate: Q1** - Explain the relationship between `__proto__`, `prototype`, and `constructor`.

<details>
<summary>Answer</summary>

These three properties form JavaScript's prototype system:

```javascript
function Person(name) {
    this.name = name;
}

const john = new Person('John');

// 1. Constructor function has 'prototype' property
console.log(Person.prototype); // Object with constructor property

// 2. prototype has 'constructor' pointing back to function
console.log(Person.prototype.constructor === Person); // true

// 3. Instance has '__proto__' pointing to constructor's prototype
console.log(john.__proto__ === Person.prototype); // true

// 4. Instance can access constructor through __proto__
console.log(john.constructor === Person); // true
console.log(john.constructor === john.__proto__.constructor); // true

// Complete relationship:
// Person (function)
//   ├── prototype ──→ Person.prototype (object)
//   │                     ├── constructor ──→ Person
//   │                     └── other methods...
//   └── __proto__ ──→ Function.prototype
//
// john (instance)
//   ├── name: 'John' (own property)
//   ├── __proto__ ──→ Person.prototype
//   └── constructor ──→ Person (inherited)

// Circular references
console.log(Person.prototype.constructor.prototype === Person.prototype); // true

// All functions have prototype property
function test() {}
console.log(test.prototype.constructor === test); // true
```
</details>

**Intermediate: Q2** - What is the difference between `Object.getPrototypeOf()` and accessing `__proto__`?

<details>
<summary>Answer</summary>

Both access the same thing, but `Object.getPrototypeOf()` is the standard way:

```javascript
function Animal(name) {
    this.name = name;
}

const dog = new Animal('Buddy');

// Both return the same object
console.log(dog.__proto__ === Object.getPrototypeOf(dog)); // true
console.log(dog.__proto__ === Animal.prototype); // true
console.log(Object.getPrototypeOf(dog) === Animal.prototype); // true

// Key differences:

// 1. Standards compliance
// __proto__ - deprecated, non-standard
// Object.getPrototypeOf() - ES5 standard

// 2. Browser support
// __proto__ - not supported in some old browsers
// Object.getPrototypeOf() - widely supported

// 3. Performance
// __proto__ - can be slower in some engines
// Object.getPrototypeOf() - optimized

// 4. Setting prototypes
// __proto__ assignment (avoid)
dog.__proto__ = {};

// Object.setPrototypeOf() (preferred)
Object.setPrototypeOf(dog, Animal.prototype);

// 5. Null prototype objects
const nullObj = Object.create(null);
console.log(nullObj.__proto__); // undefined
console.log(Object.getPrototypeOf(nullObj)); // null

// Best practice
function getProto(obj) {
    return Object.getPrototypeOf(obj);
}

function setProto(obj, proto) {
    Object.setPrototypeOf(obj, proto);
}
```
</details>

## this keyword

**Beginner: Q1** - What does the `this` keyword refer to in different contexts?

<details>
<summary>Answer</summary>

`this` refers to different objects depending on how and where it's called:

```javascript
// 1. Global context
console.log(this); // Window (browser) or global (Node.js)

// 2. Object method
const obj = {
    name: 'John',
    greet() {
        console.log(this.name); // 'John' - this refers to obj
    }
};

// 3. Constructor function
function Person(name) {
    this.name = name; // this refers to new instance
}

// 4. Event handler
button.addEventListener('click', function() {
    console.log(this); // this refers to button element
});

// 5. Arrow functions (lexical this)
const arrowObj = {
    name: 'Jane',
    greet: () => {
        console.log(this.name); // undefined - this from outer scope
    }
};

// 6. Call/apply/bind
function sayHello() {
    console.log(this.name);
}
sayHello.call({ name: 'Alice' }); // 'Alice'
```
</details>

**Beginner: Q2** - What will `this` refer to inside a regular function vs an arrow function?

<details>
<summary>Answer</summary>

Regular functions have dynamic `this`, arrow functions have lexical `this`:

```javascript
const obj = {
    name: 'John',
    
    // Regular function - dynamic this
    regularMethod: function() {
        console.log(this.name); // 'John' - this is obj
        
        function innerFunction() {
            console.log(this.name); // undefined - this is global/window
        }
        innerFunction();
    },
    
    // Arrow function - lexical this
    arrowMethod: () => {
        console.log(this.name); // undefined - this from outer scope (global)
    },
    
    // Mixed approach
    mixedMethod: function() {
        console.log(this.name); // 'John' - this is obj
        
        const arrowInside = () => {
            console.log(this.name); // 'John' - inherits this from mixedMethod
        };
        arrowInside();
    }
};

obj.regularMethod(); // 'John', undefined
obj.arrowMethod(); // undefined
obj.mixedMethod(); // 'John', 'John'

// Key difference:
// Regular function: this determined at call time
// Arrow function: this inherited from enclosing scope
```
</details>

**Beginner: Q3** - How does the value of `this` change when a method is called vs when it's assigned to a variable?

<details>
<summary>Answer</summary>

Method call vs function reference affects `this` binding:

```javascript
const person = {
    name: 'John',
    greet: function() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

// Method call - this refers to person
person.greet(); // "Hello, I'm John"

// Assigned to variable - this refers to global/window
const greetFunc = person.greet;
greetFunc(); // "Hello, I'm undefined" (strict mode: TypeError)

// Solutions:
// 1. Use bind
const boundGreet = person.greet.bind(person);
boundGreet(); // "Hello, I'm John"

// 2. Use call
greetFunc.call(person); // "Hello, I'm John"

// 3. Arrow function wrapper
const wrappedGreet = () => person.greet();
wrappedGreet(); // "Hello, I'm John"

// Common scenario - event handlers
button.addEventListener('click', person.greet); // this = button
button.addEventListener('click', person.greet.bind(person)); // this = person
```
</details>

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

<details>
<summary>Answer</summary>

Output:
```
John
undefined
```

Explanation:

```javascript
const obj = {
    name: 'John',
    greet: function() {
        console.log(this.name); // 'John'
    },
    greetArrow: () => {
        console.log(this.name); // undefined
    }
};

// obj.greet() - regular function
// this refers to obj, so this.name = 'John'
obj.greet(); // "John"

// obj.greetArrow() - arrow function
// Arrow functions don't have their own this
// this is inherited from outer scope (global scope)
// In global scope, this.name is undefined
obj.greetArrow(); // undefined

// To fix arrow function:
const obj2 = {
    name: 'Jane',
    init: function() {
        this.greetArrow = () => {
            console.log(this.name); // Now this refers to obj2
        };
    }
};

obj2.init();
obj2.greetArrow(); // "Jane"
```
</details>

**Intermediate: Q2** - How does `this` behave in strict mode vs non-strict mode?

<details>
<summary>Answer</summary>

Strict mode affects `this` binding in functions:

```javascript
// Non-strict mode
function regularFunction() {
    console.log(this); // Window object (browser)
}

regularFunction(); // Window

// Strict mode
'use strict';
function strictFunction() {
    console.log(this); // undefined
}

strictFunction(); // undefined

// Object methods - same in both modes
const obj = {
    method: function() {
        'use strict';
        console.log(this); // obj (same in both modes)
    }
};

obj.method(); // obj

// Constructor functions
function Person(name) {
    'use strict';
    this.name = name; // Works fine with 'new'
}

new Person('John'); // Creates person instance

// Calling constructor without 'new'
// Non-strict: this = window (creates global variables)
// Strict: this = undefined (throws TypeError)

function StrictConstructor(name) {
    'use strict';
    this.name = name; // TypeError if called without 'new'
}

// StrictConstructor('John'); // TypeError in strict mode

// Key differences:
// 1. Function calls: this = window vs undefined
// 2. Safety: strict mode prevents accidental global variables
// 3. Constructor calls: strict mode throws error if 'new' forgotten
```
</details>

## call, apply, bind

**Beginner: Q1** - What is the difference between `call()`, `apply()`, and `bind()`?

<details>
<summary>Answer</summary>

All three methods control `this` context, but differ in execution and parameters:

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting} ${this.name}${punctuation}`);
}

const person = { name: 'John' };

// call() - executes immediately, parameters individually
greet.call(person, 'Hello', '!'); // "Hello John!"

// apply() - executes immediately, parameters as array
greet.apply(person, ['Hi', '?']); // "Hi John?"

// bind() - returns new function, doesn't execute
const boundGreet = greet.bind(person, 'Hey');
boundGreet('!!!'); // "Hey John!!!" (executed later)

// Summary:
// call:  fn.call(thisArg, arg1, arg2, ...)
// apply: fn.apply(thisArg, [arg1, arg2, ...])
// bind:  fn.bind(thisArg, arg1, arg2, ...) -> returns function
```
</details>

**Beginner: Q2** - How do you use `call()` to invoke a function with a specific `this` value?

<details>
<summary>Answer</summary>

`call()` immediately invokes a function with specified `this` and arguments:

```javascript
function introduce() {
    console.log(`I'm ${this.name}, ${this.age} years old`);
}

const person1 = { name: 'Alice', age: 25 };
const person2 = { name: 'Bob', age: 30 };

// Use call to set this context
introduce.call(person1); // "I'm Alice, 25 years old"
introduce.call(person2); // "I'm Bob, 30 years old"

// With parameters
function greetWith(greeting, time) {
    console.log(`${greeting} ${this.name}! Good ${time}!`);
}

greetWith.call(person1, 'Hello', 'morning'); // "Hello Alice! Good morning!"

// Common use case - borrowing methods
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

// Borrow push method
Array.prototype.push.call(array1, ...array2);
console.log(array1); // [1, 2, 3, 4, 5, 6]

// Convert array-like objects
function example() {
    const args = Array.prototype.slice.call(arguments);
    console.log(args); // [1, 2, 3]
}
example(1, 2, 3);
```
</details>

**Beginner: Q3** - When would you use `bind()` instead of `call()` or `apply()`?

<details>
<summary>Answer</summary>

Use `bind()` when you need a function for later execution with fixed `this`:

```javascript
const person = {
    name: 'John',
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

// Problem: losing this context
const greetFunc = person.greet;
greetFunc(); // "Hello, I'm undefined"

// Solution: bind creates new function with fixed this
const boundGreet = person.greet.bind(person);
boundGreet(); // "Hello, I'm John"

// Use cases:

// 1. Event handlers
button.addEventListener('click', person.greet.bind(person));

// 2. Callback functions
setTimeout(person.greet.bind(person), 1000);

// 3. Partial application
function multiply(a, b) {
    return a * b;
}

const double = multiply.bind(null, 2);
console.log(double(5)); // 10

// 4. Method borrowing for later use
const logger = console.log.bind(console, '[LOG]');
logger('This is a message'); // "[LOG] This is a message"

// bind vs call/apply:
// bind: Returns function for later execution
// call/apply: Execute immediately
```
</details>

**Intermediate: Q1** - Implement your own version of the `bind()` method.

<details>
<summary>Answer</summary>

```javascript
// Custom bind implementation
Function.prototype.myBind = function(context, ...bindArgs) {
    const originalFunction = this;
    
    return function(...callArgs) {
        // Combine bind-time args with call-time args
        const allArgs = [...bindArgs, ...callArgs];
        
        // Use apply to set context and pass arguments
        return originalFunction.apply(context, allArgs);
    };
};

// Test implementation
function greet(greeting, punctuation) {
    console.log(`${greeting} ${this.name}${punctuation}`);
}

const person = { name: 'John' };

// Using custom bind
const boundGreet = greet.myBind(person, 'Hello');
boundGreet('!'); // "Hello John!"

// More advanced version handling 'new' operator
Function.prototype.myBindAdvanced = function(context, ...bindArgs) {
    const fn = this;
    
    function bound(...callArgs) {
        const allArgs = [...bindArgs, ...callArgs];
        
        // Check if called with 'new'
        if (new.target) {
            // Create new instance and call original constructor
            return new fn(...allArgs);
        }
        
        return fn.apply(context, allArgs);
    }
    
    // Maintain prototype chain
    bound.prototype = Object.create(fn.prototype);
    
    return bound;
};

// Test with constructor
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const BoundPerson = Person.myBindAdvanced(null, 'John');
const john = new BoundPerson(25);
console.log(john.name); // "John"
```
</details>

**Intermediate: Q2** - What will be the output of this code?
```javascript
function greet() {
    console.log(`${this.greeting} ${this.name}`);
}
const person = { greeting: 'Hello', name: 'John' };
const boundGreet = greet.bind(person);
boundGreet.call({ greeting: 'Hi', name: 'Jane' });
```

<details>
<summary>Answer</summary>

Output: `"Hello John"`

Explanation:

```javascript
function greet() {
    console.log(`${this.greeting} ${this.name}`);
}
const person = { greeting: 'Hello', name: 'John' };
const boundGreet = greet.bind(person);

// bind() permanently sets the this context
// Once bound, the this context cannot be changed by call(), apply(), or bind()

boundGreet.call({ greeting: 'Hi', name: 'Jane' });
// Output: "Hello John" (not "Hi Jane")

// The call() method tries to change this to { greeting: 'Hi', name: 'Jane' }
// But bind() has already locked this to { greeting: 'Hello', name: 'John' }

// Demonstration:
const unboundGreet = greet;
unboundGreet.call({ greeting: 'Hi', name: 'Jane' }); // "Hi Jane"

const boundGreet2 = greet.bind(person);
boundGreet2.call({ greeting: 'Hi', name: 'Jane' }); // "Hello John"
boundGreet2.apply({ greeting: 'Hey', name: 'Bob' }); // "Hello John"

// Key point: bind() creates a permanently bound function
// Subsequent call/apply/bind attempts are ignored
```
</details>

## Object.keys, values, entries

**Beginner: Q1** - What do `Object.keys()`, `Object.values()`, and `Object.entries()` return?

<details>
<summary>Answer</summary>

These methods extract different parts of an object:

```javascript
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
};

// Object.keys() - returns array of property names
console.log(Object.keys(person)); // ['name', 'age', 'city']

// Object.values() - returns array of property values
console.log(Object.values(person)); // ['John', 30, 'New York']

// Object.entries() - returns array of [key, value] pairs
console.log(Object.entries(person)); 
// [['name', 'John'], ['age', 30], ['city', 'New York']]

// All return arrays, not the original object
// All only include own enumerable properties (not inherited)

const obj = { a: 1, b: 2 };
Object.defineProperty(obj, 'c', { 
    value: 3, 
    enumerable: false 
});

console.log(Object.keys(obj)); // ['a', 'b'] (c is not enumerable)
```
</details>

**Beginner: Q2** - How would you iterate over an object's properties using these methods?

<details>
<summary>Answer</summary>

Each method enables different iteration patterns:

```javascript
const user = {
    name: 'Alice',
    age: 25,
    email: 'alice@example.com'
};

// 1. Iterate over keys
Object.keys(user).forEach(key => {
    console.log(`Key: ${key}`);
});

// 2. Iterate over values
Object.values(user).forEach(value => {
    console.log(`Value: ${value}`);
});

// 3. Iterate over key-value pairs
Object.entries(user).forEach(([key, value]) => {
    console.log(`${key}: ${value}`);
});

// Using for...of loop
for (const key of Object.keys(user)) {
    console.log(`${key}: ${user[key]}`);
}

for (const [key, value] of Object.entries(user)) {
    console.log(`${key}: ${value}`);
}

// Map over object properties
const uppercaseKeys = Object.keys(user).map(key => key.toUpperCase());
const doubledValues = Object.values(user).map(val => 
    typeof val === 'number' ? val * 2 : val
);

// Convert back to object
const uppercasedObj = Object.fromEntries(
    Object.entries(user).map(([key, value]) => [key.toUpperCase(), value])
);
```
</details>

**Beginner: Q3** - Do these methods include inherited properties?

<details>
<summary>Answer</summary>

No, these methods only return own enumerable properties, not inherited ones:

```javascript
// Parent object
const parent = {
    inherited: 'I am inherited'
};

// Child object
const child = Object.create(parent);
child.own = 'I am own property';

console.log(child.inherited); // 'I am inherited' (accessible)
console.log(child.own); // 'I am own property'

// Object methods only return own properties
console.log(Object.keys(child)); // ['own'] (inherited not included)
console.log(Object.values(child)); // ['I am own property']
console.log(Object.entries(child)); // [['own', 'I am own property']]

// To get inherited properties, use for...in
console.log('Using for...in:');
for (const key in child) {
    console.log(key); // 'own', 'inherited'
}

// Check if property is own vs inherited
for (const key in child) {
    if (child.hasOwnProperty(key)) {
        console.log(`Own: ${key}`);
    } else {
        console.log(`Inherited: ${key}`);
    }
}

// Get all property names (including inherited)
function getAllKeys(obj) {
    const keys = [];
    for (const key in obj) {
        keys.push(key);
    }
    return keys;
}

console.log(getAllKeys(child)); // ['own', 'inherited']
```
</details>

**Intermediate: Q1** - What's the difference between `for...in` loop and `Object.keys()`?

<details>
<summary>Answer</summary>

Key differences in property enumeration:

```javascript
const parent = { inherited: 'parent property' };
const child = Object.create(parent);
child.own = 'child property';

// Add non-enumerable property
Object.defineProperty(child, 'hidden', {
    value: 'not enumerable',
    enumerable: false
});

// 1. for...in - includes inherited enumerable properties
console.log('for...in:');
for (const key in child) {
    console.log(key); // 'own', 'inherited'
}

// 2. Object.keys() - only own enumerable properties
console.log('Object.keys():');
console.log(Object.keys(child)); // ['own']

// Comparison table:
//                    | for...in | Object.keys()
// Own properties    |    ✓     |      ✓
// Inherited props   |    ✓     |      ✗
// Non-enumerable    |    ✗     |      ✗
// Returns array     |    ✗     |      ✓

// Filter for...in to own properties only
for (const key in child) {
    if (child.hasOwnProperty(key)) {
        console.log(`Own: ${key}`);
    }
}

// Get all own properties (including non-enumerable)
console.log(Object.getOwnPropertyNames(child)); // ['own', 'hidden']

// Performance: Object.keys() is generally faster
// Use case: 
// - for...in: When you need inherited properties
// - Object.keys(): When you only want own properties
```
</details>

**Intermediate: Q2** - How would you convert an array of key-value pairs back to an object?

<details>
<summary>Answer</summary>

Use `Object.fromEntries()` to convert key-value pairs back to an object:

```javascript
// Array of key-value pairs
const entries = [
    ['name', 'John'],
    ['age', 30],
    ['city', 'New York']
];

// Convert to object
const obj = Object.fromEntries(entries);
console.log(obj); // { name: 'John', age: 30, city: 'New York' }

// Round trip: object -> entries -> object
const original = { a: 1, b: 2, c: 3 };
const converted = Object.fromEntries(Object.entries(original));
console.log(converted); // { a: 1, b: 2, c: 3 }

// Practical examples:

// 1. Transform object values
const prices = { apple: 1.2, banana: 0.8, orange: 1.5 };
const discounted = Object.fromEntries(
    Object.entries(prices).map(([fruit, price]) => [fruit, price * 0.9])
);

// 2. Filter object properties
const user = { name: 'John', age: 30, password: 'secret' };
const publicUser = Object.fromEntries(
    Object.entries(user).filter(([key]) => key !== 'password')
);

// 3. Convert Map to object
const map = new Map([['x', 1], ['y', 2]]);
const objFromMap = Object.fromEntries(map);

// 4. Manual implementation (before Object.fromEntries)
function fromEntries(entries) {
    const obj = {};
    entries.forEach(([key, value]) => {
        obj[key] = value;
    });
    return obj;
}
```
</details>

## Object.freeze, seal, assign

**Beginner: Q1** - What does `Object.freeze()` do? Can you modify a frozen object?

<details>
<summary>Answer</summary>

`Object.freeze()` makes an object immutable - no modifications allowed:

```javascript
const obj = { name: 'John', age: 30 };

// Freeze the object
Object.freeze(obj);

// Attempts to modify will fail silently (or throw in strict mode)
obj.name = 'Jane'; // Ignored
obj.city = 'NYC'; // Ignored
delete obj.age; // Ignored

console.log(obj); // { name: 'John', age: 30 } - unchanged

// Check if object is frozen
console.log(Object.isFrozen(obj)); // true

// In strict mode, modifications throw errors
'use strict';
// obj.name = 'Jane'; // TypeError: Cannot assign to read only property

// Freeze is shallow - nested objects can still be modified
const user = {
    name: 'John',
    address: { city: 'NYC', zip: 10001 }
};

Object.freeze(user);
user.address.city = 'LA'; // This works!
console.log(user.address.city); // 'LA'
```
</details>

**Beginner: Q2** - What is the difference between `Object.freeze()` and `Object.seal()`?

<details>
<summary>Answer</summary>

Both prevent modifications, but `seal()` allows changing existing property values:

```javascript
const frozenObj = { a: 1, b: 2 };
const sealedObj = { a: 1, b: 2 };

Object.freeze(frozenObj);
Object.seal(sealedObj);

// Modifying existing properties
frozenObj.a = 10; // Ignored
sealedObj.a = 10; // Works!

console.log(frozenObj.a); // 1 (unchanged)
console.log(sealedObj.a); // 10 (changed)

// Adding new properties
frozenObj.c = 3; // Ignored
sealedObj.c = 3; // Ignored

// Deleting properties
delete frozenObj.b; // Ignored
delete sealedObj.b; // Ignored

// Comparison table:
//                    | freeze() | seal()
// Modify existing    |    ✗     |   ✓
// Add new props      |    ✗     |   ✗
// Delete props       |    ✗     |   ✗
// Configure props    |    ✗     |   ✗

// Check methods
console.log(Object.isFrozen(frozenObj)); // true
console.log(Object.isSealed(sealedObj)); // true
console.log(Object.isSealed(frozenObj)); // true (frozen is also sealed)
```
</details>

**Beginner: Q3** - How do you copy properties from one object to another?

<details>
<summary>Answer</summary>

Several ways to copy object properties:

```javascript
const source = { a: 1, b: 2 };
const target = { c: 3 };

// 1. Object.assign() - modifies target
Object.assign(target, source);
console.log(target); // { c: 3, a: 1, b: 2 }

// 2. Spread operator - creates new object
const combined = { ...target, ...source };
console.log(combined); // { c: 3, a: 1, b: 2 }

// 3. Multiple sources
const source1 = { a: 1 };
const source2 = { b: 2 };
const result = Object.assign({}, source1, source2);

// 4. Copy with overrides
const user = { name: 'John', age: 30 };
const updated = { ...user, age: 31, city: 'NYC' };
console.log(updated); // { name: 'John', age: 31, city: 'NYC' }

// 5. Manual copying
function copyProperties(target, source) {
    for (const key in source) {
        if (source.hasOwnProperty(key)) {
            target[key] = source[key];
        }
    }
    return target;
}

// Note: All these methods create shallow copies
const original = { data: { count: 1 } };
const copy = { ...original };
copy.data.count = 2;
console.log(original.data.count); // 2 (shared reference!)
```
</details>

**Intermediate: Q1** - What will happen in this code?
```javascript
const obj = { a: 1, b: { c: 2 } };
Object.freeze(obj);
obj.b.c = 3;
console.log(obj.b.c);
```

<details>
<summary>Answer</summary>

Output: `3`

Explanation - `Object.freeze()` is shallow, nested objects remain mutable:

```javascript
const obj = { a: 1, b: { c: 2 } };
Object.freeze(obj);

// This fails - top-level property is frozen
obj.a = 10; // Ignored
console.log(obj.a); // 1

// This works - nested object is NOT frozen
obj.b.c = 3; // Success!
console.log(obj.b.c); // 3

// The nested object 'b' itself is still mutable
obj.b.newProp = 'new'; // Works!
console.log(obj.b); // { c: 3, newProp: 'new' }

// To freeze deeply, you need recursive freezing
function deepFreeze(obj) {
    Object.getOwnPropertyNames(obj).forEach(prop => {
        if (obj[prop] !== null && typeof obj[prop] === 'object') {
            deepFreeze(obj[prop]);
        }
    });
    return Object.freeze(obj);
}

const deepObj = { a: 1, b: { c: 2 } };
deepFreeze(deepObj);
deepObj.b.c = 99; // Now this is ignored
console.log(deepObj.b.c); // 2 (unchanged)
```
</details>

**Intermediate: Q2** - How is `Object.assign()` different from the spread operator for object copying?

<details>
<summary>Answer</summary>

Both perform shallow copying but have different behaviors:

```javascript
const source = { a: 1, b: 2 };
const target = { c: 3 };

// 1. Object.assign() - mutates target object
Object.assign(target, source);
console.log(target); // { c: 3, a: 1, b: 2 } - target modified

// 2. Spread operator - creates new object
const target2 = { c: 3 };
const result = { ...target2, ...source };
console.log(target2); // { c: 3 } - target2 unchanged
console.log(result); // { c: 3, a: 1, b: 2 } - new object

// Key differences:

// Mutation:
const original = { x: 1 };
Object.assign(original, { y: 2 }); // Mutates original
const spread = { ...original, z: 3 }; // Creates new object

// Getters/Setters:
const obj = {
    get prop() { return this._prop; },
    set prop(value) { this._prop = value; }
};

// Object.assign copies getter result, not getter itself
const assigned = Object.assign({}, obj);
console.log(assigned); // { prop: undefined }

// Spread copies descriptors
const spreaded = { ...obj }; // Also copies getter result

// Performance:
// Object.assign: Generally faster for large objects
// Spread: More readable, functional style

// Use cases:
// Object.assign: When you want to mutate existing object
// Spread: When you want immutable operations
```
</details>

## Shallow vs deep copy

**Beginner: Q1** - What is the difference between shallow copy and deep copy?

<details>
<summary>Answer</summary>

- **Shallow copy**: Copies only the first level, nested objects share references
- **Deep copy**: Copies all levels, completely independent objects

```javascript
const original = {
    name: 'John',
    address: {
        city: 'NYC',
        zip: 10001
    },
    hobbies: ['reading', 'coding']
};

// Shallow copy
const shallowCopy = { ...original };

// Modify nested object
shallowCopy.address.city = 'LA';
shallowCopy.hobbies.push('gaming');

console.log(original.address.city); // 'LA' - changed!
console.log(original.hobbies); // ['reading', 'coding', 'gaming'] - changed!

// Deep copy (simple method)
const deepCopy = JSON.parse(JSON.stringify(original));

// Modify nested object
deepCopy.address.city = 'Chicago';
deepCopy.hobbies.push('swimming');

console.log(original.address.city); // 'NYC' - unchanged
console.log(original.hobbies); // ['reading', 'coding'] - unchanged

// Visual representation:
// Shallow: obj1.nested → [same object] ← obj2.nested
// Deep:    obj1.nested → [object A], obj2.nested → [object B]
```
</details>

**Beginner: Q2** - Give an example where shallow copying would cause issues.

<details>
<summary>Answer</summary>

Shallow copying causes unintended side effects when modifying nested data:

```javascript
// Problem scenario: User preferences
const defaultSettings = {
    theme: 'light',
    notifications: {
        email: true,
        sms: false,
        push: true
    },
    permissions: ['read', 'write']
};

// Create user settings with shallow copy
const userSettings = { ...defaultSettings };

// User changes notification preferences
userSettings.notifications.email = false;
userSettings.permissions.push('admin');

// BUG: Default settings are also modified!
console.log(defaultSettings.notifications.email); // false (should be true)
console.log(defaultSettings.permissions); // ['read', 'write', 'admin']

// This affects all other users who use defaultSettings!

// Another example: Shopping cart items
const product = {
    id: 1,
    name: 'Laptop',
    specs: { ram: '8GB', storage: '256GB' }
};

const cart = [];
const item1 = { ...product, quantity: 1 };
const item2 = { ...product, quantity: 2 };

// Customize item1
item1.specs.ram = '16GB'; // Modifies original product specs!

console.log(item2.specs.ram); // '16GB' (should be '8GB')

// Solution: Deep copy
function deepCopy(obj) {
    return JSON.parse(JSON.stringify(obj));
}

const safeItem1 = { ...deepCopy(product), quantity: 1 };
```
</details>

**Beginner: Q3** - How would you create a shallow copy of an object?

<details>
<summary>Answer</summary>

Multiple methods for shallow copying:

```javascript
const original = { a: 1, b: { c: 2 } };

// 1. Spread operator (most common)
const copy1 = { ...original };

// 2. Object.assign()
const copy2 = Object.assign({}, original);

// 3. Object.create() with descriptors
const copy3 = Object.create(
    Object.getPrototypeOf(original),
    Object.getOwnPropertyDescriptors(original)
);

// 4. Manual copying
const copy4 = {};
for (const key in original) {
    if (original.hasOwnProperty(key)) {
        copy4[key] = original[key];
    }
}

// All create shallow copies
copy1.b.c = 99;
console.log(original.b.c); // 99 (modified original!)

// For arrays:
const arr = [1, { x: 2 }, 3];

// Array shallow copy methods
const arrCopy1 = [...arr];
const arrCopy2 = arr.slice();
const arrCopy3 = Array.from(arr);
const arrCopy4 = arr.concat();

// All have same shallow copy behavior
arrCopy1[1].x = 99;
console.log(arr[1].x); // 99 (modified original!)
```
</details>

**Intermediate: Q1** - Implement a function to create a deep copy of an object.

<details>
<summary>Answer</summary>

```javascript
function deepCopy(obj) {
    // Handle primitive types and null
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    
    // Handle Date
    if (obj instanceof Date) {
        return new Date(obj.getTime());
    }
    
    // Handle Array
    if (Array.isArray(obj)) {
        return obj.map(item => deepCopy(item));
    }
    
    // Handle Object
    const copy = {};
    for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
            copy[key] = deepCopy(obj[key]);
        }
    }
    
    return copy;
}

// Advanced version handling more types
function advancedDeepCopy(obj, visited = new WeakMap()) {
    // Handle primitives and null
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    
    // Handle circular references
    if (visited.has(obj)) {
        return visited.get(obj);
    }
    
    // Handle Date
    if (obj instanceof Date) {
        return new Date(obj.getTime());
    }
    
    // Handle RegExp
    if (obj instanceof RegExp) {
        return new RegExp(obj.source, obj.flags);
    }
    
    // Handle Array
    if (Array.isArray(obj)) {
        const copy = [];
        visited.set(obj, copy);
        obj.forEach((item, index) => {
            copy[index] = advancedDeepCopy(item, visited);
        });
        return copy;
    }
    
    // Handle Object
    const copy = {};
    visited.set(obj, copy);
    
    for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
            copy[key] = advancedDeepCopy(obj[key], visited);
        }
    }
    
    return copy;
}

// Test with complex object
const complex = {
    date: new Date(),
    regex: /test/gi,
    nested: { deep: { value: 42 } },
    array: [1, { x: 2 }]
};

const copied = advancedDeepCopy(complex);
copied.nested.deep.value = 99;
console.log(complex.nested.deep.value); // 42 (unchanged)
```
</details>

**Intermediate: Q2** - What are the limitations of using `JSON.parse(JSON.stringify())` for deep copying?

<details>
<summary>Answer</summary>

`JSON.parse(JSON.stringify())` has several limitations:

```javascript
const obj = {
    // 1. Functions are lost
    method: function() { return 'hello'; },
    
    // 2. undefined values are lost
    undefined: undefined,
    
    // 3. Symbol keys/values are lost
    [Symbol('key')]: 'symbol value',
    symbolValue: Symbol('test'),
    
    // 4. Date becomes string
    date: new Date(),
    
    // 5. RegExp becomes empty object
    regex: /test/gi,
    
    // 6. Infinity/NaN become null
    infinity: Infinity,
    nan: NaN,
    
    // 7. Only enumerable properties
    normal: 'value'
};

// Add non-enumerable property
Object.defineProperty(obj, 'hidden', {
    value: 'not copied',
    enumerable: false
});

const copy = JSON.parse(JSON.stringify(obj));

console.log(copy);
// Result: { date: "2023-...", regex: {}, infinity: null, nan: null, normal: "value" }
// Missing: method, undefined, Symbol properties, hidden property

// 8. Circular references cause errors
const circular = { a: 1 };
circular.self = circular;

// JSON.stringify(circular); // TypeError: Converting circular structure

// 9. Prototype chain is lost
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function() { return `Hello ${this.name}`; };

const person = new Person('John');
const personCopy = JSON.parse(JSON.stringify(person));

console.log(person.greet()); // "Hello John"
// console.log(personCopy.greet()); // TypeError: not a function

// When JSON method is acceptable:
// - Plain objects with primitive values
// - No functions, dates, or special objects
// - No circular references
// - Performance is critical (JSON is faster)
```
</details>

## Arrow functions

**Beginner: Q1** - What is the syntax for arrow functions? Give examples of different forms.

<details>
<summary>Answer</summary>

Arrow functions have multiple syntax forms:

```javascript
// 1. Basic syntax
const add = (a, b) => a + b;

// 2. Single parameter (parentheses optional)
const square = x => x * x;
const squareWithParens = (x) => x * x;

// 3. No parameters
const sayHello = () => 'Hello!';

// 4. Multiple statements (requires braces and return)
const calculate = (x, y) => {
    const sum = x + y;
    const product = x * y;
    return { sum, product };
};

// 5. Returning object literal (wrap in parentheses)
const createUser = (name, age) => ({ name, age });

// 6. Multi-line arrow function
const processData = (data) => {
    if (!data) return null;
    return data
        .filter(item => item.active)
        .map(item => item.name)
        .join(', ');
};

// 7. Higher-order functions
const multiply = (factor) => (number) => number * factor;
const double = multiply(2);
console.log(double(5)); // 10

// 8. Array methods
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter(n => n % 2 === 0);
const doubled = numbers.map(n => n * 2);

// Regular function equivalent
function addRegular(a, b) {
    return a + b;
}
```
</details>

**Beginner: Q2** - How do arrow functions handle the `this` keyword differently from regular functions?

<details>
<summary>Answer</summary>

Arrow functions inherit `this` from enclosing scope (lexical `this`):

```javascript
const obj = {
    name: 'John',
    
    // Regular function - dynamic this
    regularMethod: function() {
        console.log(this.name); // 'John'
        
        setTimeout(function() {
            console.log(this.name); // undefined (this = window/global)
        }, 100);
    },
    
    // Arrow function - lexical this
    arrowMethod: function() {
        console.log(this.name); // 'John'
        
        setTimeout(() => {
            console.log(this.name); // 'John' (inherits from arrowMethod)
        }, 100);
    }
};

// Event handlers
class Button {
    constructor(element) {
        this.element = element;
        this.clickCount = 0;
        
        // Problem with regular function
        this.element.addEventListener('click', function() {
            this.clickCount++; // this = element, not Button instance
        });
        
        // Solution with arrow function
        this.element.addEventListener('click', () => {
            this.clickCount++; // this = Button instance
        });
    }
}

// Class methods
class Counter {
    constructor() {
        this.count = 0;
    }
    
    increment() {
        this.count++; // this = Counter instance
    }
    
    // Arrow function as property
    incrementArrow = () => {
        this.count++; // this = Counter instance (always)
    }
}

const counter = new Counter();
const incrementFunc = counter.increment;
incrementFunc(); // this = undefined (lost context)

const incrementArrowFunc = counter.incrementArrow;
incrementArrowFunc(); // this = counter (preserved context)
```
</details>

**Beginner: Q3** - Can arrow functions be used as constructors?

<details>
<summary>Answer</summary>

No, arrow functions cannot be used as constructors:

```javascript
// Regular function - works as constructor
function Person(name) {
    this.name = name;
}

const person1 = new Person('John'); // Works

// Arrow function - cannot be constructor
const PersonArrow = (name) => {
    this.name = name;
};

// const person2 = new PersonArrow('Jane'); // TypeError: PersonArrow is not a constructor

// Why arrow functions can't be constructors:
// 1. No prototype property
console.log(Person.prototype); // { constructor: Person }
console.log(PersonArrow.prototype); // undefined

// 2. No own 'this' binding
// Regular functions get new 'this' when called with 'new'
// Arrow functions always use lexical 'this'

// 3. No 'new.target'
function RegularFunc() {
    console.log(new.target); // Shows constructor when called with 'new'
}

const ArrowFunc = () => {
    // console.log(new.target); // SyntaxError in arrow function
};

// Alternatives for object creation:
// 1. Factory function
const createPerson = (name) => ({ name });

// 2. Class with arrow methods
class PersonClass {
    constructor(name) {
        this.name = name;
    }
    
    // Arrow method for 'this' binding
    greet = () => {
        return `Hello, I'm ${this.name}`;
    }
}

// 3. Object literal
const personObj = {
    name: 'John',
    greet: () => 'Hello!' // Note: arrow function here loses 'this'
};
```
</details>

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

<details>
<summary>Answer</summary>

Output: `"Test"`

Explanation:

```javascript
const obj = {
    name: 'Test',
    getName: function() {
        // Regular function: this = obj
        console.log('Outer this.name:', this.name); // "Test"
        
        const inner = () => {
            // Arrow function: inherits this from getName()
            // So this = obj (same as outer function)
            return this.name;
        };
        return inner();
    }
};

console.log(obj.getName()); // "Test"

// Step-by-step execution:
// 1. obj.getName() is called
// 2. getName() is a regular function, so this = obj
// 3. inner is an arrow function defined inside getName()
// 4. Arrow functions inherit this from enclosing scope
// 5. So inner's this = getName's this = obj
// 6. this.name = obj.name = "Test"

// Compare with regular function inside:
const obj2 = {
    name: 'Test',
    getName: function() {
        function inner() {
            return this.name; // this = undefined (strict) or window
        }
        return inner();
    }
};

console.log(obj2.getName()); // undefined (strict mode)

// Fix with bind:
const obj3 = {
    name: 'Test',
    getName: function() {
        function inner() {
            return this.name;
        }
        return inner.call(this); // or inner.bind(this)()
    }
};
```
</details>

**Intermediate: Q2** - When should you use arrow functions and when should you avoid them?

<details>
<summary>Answer</summary>

**Use arrow functions when:**

```javascript
// 1. Array methods and functional programming
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);

// 2. Callbacks that need to preserve 'this'
class Timer {
    constructor() {
        this.seconds = 0;
        setInterval(() => {
            this.seconds++; // 'this' refers to Timer instance
        }, 1000);
    }
}

// 3. Short, simple functions
const add = (a, b) => a + b;
const isEven = n => n % 2 === 0;

// 4. Promise chains
fetch('/api/data')
    .then(response => response.json())
    .then(data => data.filter(item => item.active))
    .then(filtered => console.log(filtered));
```

**Avoid arrow functions when:**

```javascript
// 1. Object methods (lose 'this' context)
const person = {
    name: 'John',
    greet: () => {
        console.log(`Hello, ${this.name}`); // 'this' is not person
    }
};

// Better:
const person2 = {
    name: 'John',
    greet() {
        console.log(`Hello, ${this.name}`); // 'this' is person2
    }
};

// 2. Event handlers needing element context
button.addEventListener('click', function() {
    this.classList.toggle('active'); // 'this' is button
});

// 3. Functions needing 'arguments' object
function sum() {
    return Array.from(arguments).reduce((a, b) => a + b, 0);
}

// Arrow functions don't have 'arguments'
// const sumArrow = () => {
//     return Array.from(arguments); // ReferenceError
// };

// 4. Constructor functions
function Person(name) {
    this.name = name; // Can use with 'new'
}

// 5. Functions needing to be hoisted
// Function declarations are hoisted, arrow functions are not
console.log(hoisted()); // Works

function hoisted() {
    return 'I am hoisted';
}

// console.log(notHoisted()); // ReferenceError
// const notHoisted = () => 'I am not hoisted';

// 6. Prototype methods
Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};
```
</details>
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

<details>
<summary>Answer</summary>

Template literals use backticks and support embedded expressions and multi-line strings:

```javascript
// Regular strings
const name = 'John';
const regular = 'Hello ' + name + '!';
const multiLine = 'Line 1\n' + 'Line 2';

// Template literals
const template = `Hello ${name}!`;
const multiLineTemplate = `Line 1
Line 2`;

// Key differences:
// 1. Syntax: backticks (`) vs quotes (' or ")
// 2. Expression embedding: ${expression}
// 3. Multi-line support without escape characters
// 4. Preserve whitespace and formatting

const user = { name: 'Alice', age: 30 };
const info = `
    Name: ${user.name}
    Age: ${user.age}
    Status: ${user.age >= 18 ? 'Adult' : 'Minor'}
`;

console.log(info);
// Output:
//     Name: Alice
//     Age: 30
//     Status: Adult
```
</details>

**Beginner: Q2** - How do you embed expressions in template literals?

<details>
<summary>Answer</summary>

Use `${expression}` syntax to embed any JavaScript expression:

```javascript
const a = 5;
const b = 10;

// Basic expressions
console.log(`Sum: ${a + b}`); // "Sum: 15"
console.log(`Product: ${a * b}`); // "Product: 50"

// Function calls
function greet(name) {
    return `Hello, ${name}`;
}
console.log(`${greet('John')}!`); // "Hello, John!"

// Object properties
const user = { name: 'Alice', age: 25 };
console.log(`User: ${user.name}, Age: ${user.age}`);

// Conditional expressions
const score = 85;
console.log(`Grade: ${score >= 90 ? 'A' : score >= 80 ? 'B' : 'C'}`);

// Array methods
const numbers = [1, 2, 3];
console.log(`Numbers: ${numbers.join(', ')}`);

// Nested template literals
const items = ['apple', 'banana'];
const list = `Items: ${items.map(item => `• ${item}`).join('\n')}`;

// Complex expressions
const price = 19.99;
const tax = 0.08;
console.log(`Total: $${(price * (1 + tax)).toFixed(2)}`);
```
</details>

**Beginner: Q3** - Can template literals span multiple lines?

<details>
<summary>Answer</summary>

Yes, template literals preserve line breaks and whitespace:

```javascript
// Multi-line template literal
const multiLine = `This is line 1
This is line 2
This is line 3`;

console.log(multiLine);
// Output:
// This is line 1
// This is line 2
// This is line 3

// HTML template
const html = `
<div class="user-card">
    <h2>${user.name}</h2>
    <p>Age: ${user.age}</p>
    <p>Email: ${user.email}</p>
</div>`;

// SQL query
const query = `
SELECT name, age, email
FROM users
WHERE age > ${minAge}
ORDER BY name ASC
`;

// Preserves indentation
const indented = `
    Level 1
        Level 2
            Level 3
`;

// Compare with regular strings (harder to read)
const regularMultiLine = 'This is line 1\n' +
                         'This is line 2\n' +
                         'This is line 3';

// Remove leading whitespace if needed
function dedent(str) {
    const lines = str.split('\n');
    const minIndent = Math.min(
        ...lines.filter(line => line.trim()).map(line => 
            line.match(/^\s*/)[0].length
        )
    );
    return lines.map(line => line.slice(minIndent)).join('\n').trim();
}

const clean = dedent(`
    Indented text
        More indented
    Back to normal
`);
```
</details>

**Intermediate: Q1** - What are tagged template literals and how do they work?

<details>
<summary>Answer</summary>

Tagged templates call a function with template literal parts and expressions:

```javascript
// Basic tagged template
function highlight(strings, ...values) {
    console.log('Strings:', strings); // Array of string parts
    console.log('Values:', values);   // Array of expressions
    
    return strings.reduce((result, string, i) => {
        const value = values[i] ? `<mark>${values[i]}</mark>` : '';
        return result + string + value;
    }, '');
}

const name = 'John';
const age = 30;
const result = highlight`Hello ${name}, you are ${age} years old!`;
// Strings: ['Hello ', ', you are ', ' years old!']
// Values: ['John', 30]
// Result: "Hello <mark>John</mark>, you are <mark>30</mark> years old!"

// Practical examples:

// 1. Safe HTML escaping
function safeHtml(strings, ...values) {
    return strings.reduce((result, string, i) => {
        const value = values[i] 
            ? String(values[i]).replace(/[&<>"']/g, match => ({
                '&': '&amp;', '<': '&lt;', '>': '&gt;',
                '"': '&quot;', "'": '&#39;'
            })[match])
            : '';
        return result + string + value;
    }, '');
}

const userInput = '<script>alert("xss")</script>';
const safe = safeHtml`<div>${userInput}</div>`;
// Result: "<div>&lt;script&gt;alert(&quot;xss&quot;)&lt;/script&gt;</div>"

// 2. CSS styling
function css(strings, ...values) {
    return strings.reduce((result, string, i) => {
        return result + string + (values[i] || '');
    }, '');
}

const color = 'blue';
const size = '16px';
const styles = css`
    color: ${color};
    font-size: ${size};
    font-weight: bold;
`;

// 3. SQL query builder
function sql(strings, ...values) {
    // Escape SQL values to prevent injection
    const escaped = values.map(value => 
        typeof value === 'string' 
            ? `'${value.replace(/'/g, "''")}'`
            : value
    );
    
    return strings.reduce((query, string, i) => {
        return query + string + (escaped[i] || '');
    }, '');
}

const userId = 123;
const query = sql`SELECT * FROM users WHERE id = ${userId}`;
```
</details>

**Intermediate: Q2** - Write a tagged template function that highlights variables in a string.

<details>
<summary>Answer</summary>

```javascript
// Simple highlight function
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        const value = values[i] !== undefined 
            ? `**${values[i]}**` 
            : '';
        return result + string + value;
    }, '');
}

// Advanced highlight with options
function createHighlighter(options = {}) {
    const { 
        prefix = '**', 
        suffix = '**', 
        transform = (value) => value 
    } = options;
    
    return function highlight(strings, ...values) {
        return strings.reduce((result, string, i) => {
            if (values[i] !== undefined) {
                const transformed = transform(values[i]);
                const highlighted = `${prefix}${transformed}${suffix}`;
                return result + string + highlighted;
            }
            return result + string;
        }, '');
    };
}

// HTML highlighter
const htmlHighlight = createHighlighter({
    prefix: '<span class="highlight">',
    suffix: '</span>',
    transform: (value) => String(value).toUpperCase()
});

// Console color highlighter
const colorHighlight = createHighlighter({
    prefix: '\x1b[33m', // Yellow color
    suffix: '\x1b[0m',  // Reset color
});

// Markdown highlighter
const markdownHighlight = createHighlighter({
    prefix: '`',
    suffix: '`',
    transform: (value) => typeof value === 'object' 
        ? JSON.stringify(value) 
        : String(value)
});

// Usage examples
const name = 'Alice';
const age = 25;
const score = 95.5;

console.log(highlight`Hello ${name}, you scored ${score}%!`);
// Output: "Hello **Alice**, you scored **95.5**%!"

console.log(htmlHighlight`User ${name} is ${age} years old`);
// Output: "User <span class="highlight">ALICE</span> is <span class="highlight">25</span> years old"

console.log(colorHighlight`Processing ${name}...`);
// Output: "Processing [yellow]Alice[reset]..." (with actual colors in terminal)

const data = { id: 1, active: true };
console.log(markdownHighlight`User data: ${data}`);
// Output: "User data: `{\"id\":1,\"active\":true}`"

// Debug highlighter
function debug(strings, ...values) {
    const result = strings.reduce((acc, string, i) => {
        const value = values[i];
        const valueInfo = value !== undefined 
            ? ` [${typeof value}: ${JSON.stringify(value)}] `
            : '';
        return acc + string + valueInfo;
    }, '');
    
    console.log('DEBUG:', result);
    return result;
}

debug`User ${name} has score ${score}`;
// Output: "DEBUG: User  [string: "Alice"]  has score  [number: 95.5] "
```
</details>

## Modules (import, export)

**Beginner: Q1** - How do you export and import functions from modules in ES6?

<details>
<summary>Answer</summary>

ES6 modules use `export` and `import` keywords:

```javascript
// math.js - Exporting functions
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

export const PI = 3.14159;

// main.js - Importing functions
import { add, multiply, PI } from './math.js';

console.log(add(2, 3)); // 5
console.log(multiply(4, 5)); // 20
console.log(PI); // 3.14159

// Alternative export syntax
function subtract(a, b) {
    return a - b;
}

function divide(a, b) {
    return a / b;
}

export { subtract, divide };

// Importing with aliases
import { subtract as sub, divide as div } from './math.js';

console.log(sub(10, 3)); // 7
console.log(div(10, 2)); // 5
```
</details>

**Beginner: Q2** - What is the difference between named exports and default exports?

<details>
<summary>Answer</summary>

Named exports allow multiple exports, default export allows one main export:

```javascript
// utils.js - Named exports
export function formatDate(date) {
    return date.toLocaleDateString();
}

export function formatTime(date) {
    return date.toLocaleTimeString();
}

export const version = '1.0.0';

// user.js - Default export
class User {
    constructor(name) {
        this.name = name;
    }
}

export default User; // One default export per module

// Mixed exports
export function validateEmail(email) {
    return email.includes('@');
}

// Importing named exports (exact names, use braces)
import { formatDate, formatTime, version } from './utils.js';

// Importing default export (any name, no braces)
import User from './user.js';
import MyUser from './user.js'; // Can rename default import

// Mixed imports
import User, { validateEmail } from './user.js';

// Export patterns:

// 1. Inline named exports
export const config = { api: 'url' };
export function helper() {}

// 2. Grouped named exports
const a = 1;
const b = 2;
export { a, b };

// 3. Default export variations
export default function() {} // Anonymous function
export default class {} // Anonymous class
export default 42; // Primitive value
export default { name: 'Object' }; // Object literal

// Key differences:
// Named: import { exact, names } from 'module'
// Default: import anyName from 'module'
```
</details>

**Beginner: Q3** - How do you import all exports from a module?

<details>
<summary>Answer</summary>

Use `import * as` syntax to import all named exports:

```javascript
// math.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }
export const PI = 3.14159;
export default function calculate() { return 'Calculator ready'; }

// Import all named exports as namespace object
import * as math from './math.js';

console.log(math.add(2, 3)); // 5
console.log(math.PI); // 3.14159
console.log(math.multiply(4, 5)); // 20

// Note: Default export is NOT included in namespace import
// Import default separately
import calculate, * as math from './math.js';
console.log(calculate()); // 'Calculator ready'

// Different import patterns:

// 1. Import everything (named exports only)
import * as utils from './utils.js';

// 2. Import specific named exports
import { add, subtract } from './math.js';

// 3. Import default only
import calculator from './math.js';

// 4. Import default + specific named
import calculator, { PI, add } from './math.js';

// 5. Import default + all named
import calculator, * as math from './math.js';

// 6. Import for side effects only (no assignments)
import './polyfills.js'; // Runs the module code

// Real-world example
// lodash.js (hypothetical)
export function debounce() {}
export function throttle() {}
export function cloneDeep() {}

// Usage
import * as _ from './lodash.js';
const debouncedFn = _.debounce(myFunction, 300);

// Or destructure from namespace
const { debounce, throttle } = await import('./lodash.js');
```
</details>

**Intermediate: Q1** - What is dynamic import and when would you use it?

<details>
<summary>Answer</summary>

Dynamic import loads modules at runtime, returns a Promise:

```javascript
// Static import (load time)
import utils from './utils.js'; // Always loaded

// Dynamic import (runtime)
const utils = await import('./utils.js'); // Loaded when needed

// Basic dynamic import
async function loadMath() {
    const math = await import('./math.js');
    console.log(math.add(2, 3)); // 5
    console.log(math.default()); // Default export
}

// Conditional loading
async function loadFeature(featureName) {
    if (featureName === 'charts') {
        const chartLib = await import('./charts.js');
        return new chartLib.Chart();
    } else if (featureName === 'maps') {
        const mapLib = await import('./maps.js');
        return new mapLib.Map();
    }
}

// Use cases:

// 1. Code splitting / lazy loading
async function handleButtonClick() {
    const { heavyFunction } = await import('./heavy-module.js');
    heavyFunction();
}

// 2. Conditional polyfills
if (!Array.prototype.includes) {
    await import('./array-includes-polyfill.js');
}

// 3. Dynamic module paths
const locale = 'en';
const translations = await import(`./locales/${locale}.js`);

// 4. Feature detection
if (supportsWebGL) {
    const { WebGLRenderer } = await import('./webgl-renderer.js');
    new WebGLRenderer();
} else {
    const { CanvasRenderer } = await import('./canvas-renderer.js');
    new CanvasRenderer();
}

// 5. Plugin system
class PluginManager {
    async loadPlugin(pluginName) {
        try {
            const plugin = await import(`./plugins/${pluginName}.js`);
            return plugin.default;
        } catch (error) {
            console.error(`Failed to load plugin: ${pluginName}`);
        }
    }
}

// 6. Route-based loading (React/Vue)
const routes = [
    {
        path: '/home',
        component: () => import('./components/Home.js')
    },
    {
        path: '/about',
        component: () => import('./components/About.js')
    }
];

// Error handling
try {
    const module = await import('./maybe-missing.js');
} catch (error) {
    console.error('Module failed to load:', error);
}

// Benefits:
// - Smaller initial bundle size
// - Faster page load
// - Load features on demand
// - Better user experience
```
</details>

**Intermediate: Q2** - How do ES6 modules differ from CommonJS modules?

<details>
<summary>Answer</summary>

Key differences between ES6 modules and CommonJS:

```javascript
// COMMONJS (Node.js)
// Exporting
function add(a, b) { return a + b; }
module.exports = { add };
// or
exports.add = add;

// Importing
const { add } = require('./math');
const math = require('./math');

// ES6 MODULES
// Exporting
export function add(a, b) { return a + b; }
export default Calculator;

// Importing
import { add } from './math.js';
import Calculator from './calculator.js';

// Key Differences:

// 1. Syntax
// CommonJS: require() / module.exports
// ES6: import / export

// 2. Loading behavior
// CommonJS: Synchronous, runtime
const fs = require('fs'); // Loaded immediately

// ES6: Asynchronous, compile-time
import fs from 'fs'; // Analyzed before execution

// 3. Static vs Dynamic
// CommonJS: Dynamic (can be conditional)
if (condition) {
    const module = require('./module'); // Works
}

// ES6: Static (must be top-level)
// if (condition) {
//     import module from './module'; // SyntaxError
// }

// 4. Live bindings
// CommonJS: Copy of value
// counter.js (CommonJS)
let count = 0;
function increment() { count++; }
module.exports = { count, increment };

// main.js
const { count, increment } = require('./counter');
increment();
console.log(count); // 0 (copy, not updated)

// ES6: Live reference
// counter.js (ES6)
export let count = 0;
export function increment() { count++; }

// main.js
import { count, increment } from './counter.js';
increment();
console.log(count); // 1 (live binding, updated)

// 5. Tree shaking
// CommonJS: No tree shaking
const utils = require('./utils'); // Entire module loaded

// ES6: Tree shaking possible
import { onlyThis } from './utils.js'; // Only used exports bundled

// 6. Interoperability
// ES6 can import CommonJS
import fs from 'fs'; // Works with Node.js modules

// CommonJS can import ES6 (with dynamic import)
const esmModule = await import('./esm-module.js');

// 7. File extensions
// CommonJS: .js (default)
// ES6: .mjs or .js with "type": "module" in package.json

// Migration example:
// CommonJS -> ES6
// Before
const express = require('express');
const { validateUser } = require('./utils');
module.exports = function createApp() {};

// After
import express from 'express';
import { validateUser } from './utils.js';
export default function createApp() {}
```
</details>

## Classes & inheritance

**Beginner: Q1** - How do you create a class in ES6? How do you create an instance?

<details>
<summary>Answer</summary>

Use `class` keyword to define a class and `new` to create instances:

```javascript
// Define a class
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    getAge() {
        return this.age;
    }
    
    // Static method
    static species() {
        return 'Homo sapiens';
    }
}

// Create instances
const person1 = new Person('Alice', 30);
const person2 = new Person('Bob', 25);

console.log(person1.greet()); // "Hello, I'm Alice"
console.log(person2.getAge()); // 25
console.log(Person.species()); // "Homo sapiens"

// Class with getters and setters
class Circle {
    constructor(radius) {
        this._radius = radius;
    }
    
    get radius() {
        return this._radius;
    }
    
    set radius(value) {
        if (value <= 0) throw new Error('Radius must be positive');
        this._radius = value;
    }
    
    get area() {
        return Math.PI * this._radius ** 2;
    }
}

const circle = new Circle(5);
console.log(circle.area); // 78.54
circle.radius = 10;
console.log(circle.area); // 314.16
```
</details>

**Beginner: Q2** - How do you implement inheritance using the `extends` keyword?

<details>
<summary>Answer</summary>

Use `extends` to create a subclass that inherits from a parent class:

```javascript
// Parent class
class Animal {
    constructor(name, type) {
        this.name = name;
        this.type = type;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
    
    getInfo() {
        return `${this.name} is a ${this.type}`;
    }
}

// Child class extends parent
class Dog extends Animal {
    constructor(name, breed) {
        super(name, 'dog'); // Call parent constructor
        this.breed = breed;
    }
    
    speak() {
        return `${this.name} barks: Woof!`;
    }
    
    wagTail() {
        return `${this.name} wags tail`;
    }
}

// Another child class
class Cat extends Animal {
    constructor(name, indoor = true) {
        super(name, 'cat');
        this.indoor = indoor;
    }
    
    speak() {
        return `${this.name} meows: Meow!`;
    }
    
    purr() {
        return `${this.name} purrs contentedly`;
    }
}

// Usage
const dog = new Dog('Buddy', 'Golden Retriever');
const cat = new Cat('Whiskers', true);

console.log(dog.speak()); // "Buddy barks: Woof!"
console.log(dog.getInfo()); // "Buddy is a dog" (inherited)
console.log(dog.wagTail()); // "Buddy wags tail"

console.log(cat.speak()); // "Whiskers meows: Meow!"
console.log(cat.purr()); // "Whiskers purrs contentedly"

// instanceof checks
console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true
console.log(cat instanceof Dog); // false
```
</details>

**Beginner: Q3** - What is the `super` keyword used for?

<details>
<summary>Answer</summary>

`super` is used to call parent class constructor and methods:

```javascript
class Vehicle {
    constructor(brand, model) {
        this.brand = brand;
        this.model = model;
    }
    
    start() {
        return `${this.brand} ${this.model} is starting`;
    }
    
    getInfo() {
        return `Vehicle: ${this.brand} ${this.model}`;
    }
}

class Car extends Vehicle {
    constructor(brand, model, doors) {
        // 1. Call parent constructor
        super(brand, model); // Must be first in constructor
        this.doors = doors;
    }
    
    start() {
        // 2. Call parent method and extend it
        const parentStart = super.start();
        return `${parentStart} with ${this.doors} doors`;
    }
    
    getInfo() {
        // 3. Override parent method using super
        const parentInfo = super.getInfo();
        return `${parentInfo}, Doors: ${this.doors}`;
    }
}

const car = new Car('Toyota', 'Camry', 4);
console.log(car.start()); // "Toyota Camry is starting with 4 doors"
console.log(car.getInfo()); // "Vehicle: Toyota Camry, Doors: 4"

// Super in static methods
class MathUtils {
    static add(a, b) {
        return a + b;
    }
}

class AdvancedMath extends MathUtils {
    static add(a, b) {
        const result = super.add(a, b); // Call parent static method
        console.log(`Adding ${a} + ${b} = ${result}`);
        return result;
    }
    
    static multiply(a, b) {
        return a * b;
    }
}

AdvancedMath.add(5, 3); // "Adding 5 + 3 = 8"

// Important: super() must be called before using 'this'
class BadExample extends Vehicle {
    constructor(brand) {
        this.brand = brand; // Error: Must call super() first
        super(brand, 'Unknown');
    }
}
```
</details>

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

<details>
<summary>Answer</summary>

Output: `"Buddy makes a sound - Woof!"`

Explanation:

```javascript
class Animal {
    constructor(name) {
        this.name = name; // Sets this.name = 'Buddy'
    }
    speak() {
        return `${this.name} makes a sound`; // "Buddy makes a sound"
    }
}

class Dog extends Animal {
    speak() {
        // super.speak() calls parent's speak method
        // Returns: "Buddy makes a sound"
        // Then concatenates with " - Woof!"
        return `${super.speak()} - Woof!`;
    }
}

const dog = new Dog('Buddy');
// 1. new Dog('Buddy') calls Animal constructor with 'Buddy'
// 2. dog.speak() calls Dog's speak method
// 3. super.speak() returns "Buddy makes a sound"
// 4. Final result: "Buddy makes a sound - Woof!"
console.log(dog.speak()); // "Buddy makes a sound - Woof!"

// Method resolution order:
// 1. Look for speak() in Dog class -> Found
// 2. Inside Dog.speak(), super.speak() looks in Animal class
// 3. Animal.speak() uses this.name (which is 'Buddy')
// 4. Returns combined result

// If Dog didn't override speak():
class QuietDog extends Animal {}
const quietDog = new QuietDog('Rex');
console.log(quietDog.speak()); // "Rex makes a sound" (uses Animal's method)
```
</details>

**Intermediate: Q2** - How do private fields work in ES6 classes?

<details>
<summary>Answer</summary>

Private fields use `#` prefix and are only accessible within the class:

```javascript
class BankAccount {
    // Private fields
    #balance = 0;
    #accountNumber;
    
    // Private method
    #generateAccountNumber() {
        return Math.random().toString(36).substr(2, 9);
    }
    
    constructor(initialBalance) {
        this.#balance = initialBalance;
        this.#accountNumber = this.#generateAccountNumber();
    }
    
    // Public methods to access private fields
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
            return this.#balance;
        }
        throw new Error('Amount must be positive');
    }
    
    withdraw(amount) {
        if (amount > 0 && amount <= this.#balance) {
            this.#balance -= amount;
            return this.#balance;
        }
        throw new Error('Insufficient funds or invalid amount');
    }
    
    getBalance() {
        return this.#balance;
    }
    
    getAccountNumber() {
        return this.#accountNumber;
    }
    
    // Private static field
    static #bankName = 'SecureBank';
    
    static getBankName() {
        return this.#bankName;
    }
}

const account = new BankAccount(1000);

console.log(account.getBalance()); // 1000
account.deposit(500);
console.log(account.getBalance()); // 1500

// Private fields are not accessible from outside
// console.log(account.#balance); // SyntaxError
// account.#generateAccountNumber(); // SyntaxError

// Even with inheritance, private fields remain private
class SavingsAccount extends BankAccount {
    constructor(initialBalance, interestRate) {
        super(initialBalance);
        this.interestRate = interestRate;
    }
    
    addInterest() {
        // Cannot directly access #balance
        // const interest = this.#balance * this.interestRate; // Error
        
        // Must use public methods
        const currentBalance = this.getBalance();
        const interest = currentBalance * this.interestRate;
        this.deposit(interest);
    }
}

// Private fields vs conventions
class OldStyle {
    constructor() {
        this._private = 'convention'; // Not truly private
    }
}

class NewStyle {
    #private = 'truly private';
    
    getPrivate() {
        return this.#private;
    }
}

const old = new OldStyle();
console.log(old._private); // 'convention' (accessible)

const newObj = new NewStyle();
// console.log(newObj.#private); // SyntaxError
console.log(newObj.getPrivate()); // 'truly private'
```
</details>

## Async/await

**Beginner: Q1** - What is `async/await` and how is it used?

<details>
<summary>Answer</summary>

`async/await` provides a cleaner way to work with Promises, making asynchronous code look synchronous:

```javascript
// Promise-based approach
function fetchUserData() {
    return fetch('/api/user')
        .then(response => response.json())
        .then(data => {
            console.log('User data:', data);
            return data;
        })
        .catch(error => {
            console.error('Error:', error);
        });
}

// async/await approach
async function fetchUserDataAsync() {
    try {
        const response = await fetch('/api/user');
        const data = await response.json();
        console.log('User data:', data);
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}

// Basic async function
async function simpleExample() {
    // await pauses execution until Promise resolves
    const result = await Promise.resolve('Hello');
    console.log(result); // "Hello"
    
    // Can await any Promise-returning function
    const data = await fetch('/api/data');
    return data;
}

// Async functions always return a Promise
async function returnsPromise() {
    return 'This becomes a resolved Promise';
}

returnsPromise().then(value => console.log(value)); // "This becomes a resolved Promise"

// Multiple awaits
async function sequential() {
    const first = await delay(1000);  // Wait 1 second
    const second = await delay(500);  // Wait 0.5 second
    return [first, second];
}

function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
```
</details>

**Beginner: Q2** - How do you handle errors in async/await?

<details>
<summary>Answer</summary>

Use `try/catch` blocks to handle errors in async/await:

```javascript
// Basic error handling
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Failed to fetch data:', error.message);
        return null; // or throw error again
    }
}

// Handling multiple operations
async function processUserData(userId) {
    try {
        // Multiple await operations
        const user = await fetchUser(userId);
        const posts = await fetchUserPosts(userId);
        const profile = await updateUserProfile(user);
        
        return { user: profile, posts };
    } catch (error) {
        // Catches errors from any of the await operations
        console.error('Error processing user data:', error);
        throw error; // Re-throw if needed
    }
}

// Specific error handling
async function robustFetch(url) {
    try {
        const response = await fetch(url);
        const data = await response.json();
        return data;
    } catch (error) {
        if (error instanceof TypeError) {
            console.error('Network error:', error.message);
        } else if (error instanceof SyntaxError) {
            console.error('JSON parsing error:', error.message);
        } else {
            console.error('Unknown error:', error.message);
        }
        
        // Return default value or re-throw
        return { error: 'Failed to fetch data' };
    }
}

// Error handling with finally
async function withCleanup() {
    let resource = null;
    
    try {
        resource = await acquireResource();
        const result = await processResource(resource);
        return result;
    } catch (error) {
        console.error('Processing failed:', error);
        return null;
    } finally {
        // Always runs, even if error occurs
        if (resource) {
            await cleanupResource(resource);
        }
    }
}

// Multiple error sources
async function complexOperation() {
    try {
        const step1 = await operation1();
        const step2 = await operation2(step1);
        const step3 = await operation3(step2);
        return step3;
    } catch (error) {
        // Handle different types of errors
        switch (error.code) {
            case 'NETWORK_ERROR':
                return handleNetworkError(error);
            case 'VALIDATION_ERROR':
                return handleValidationError(error);
            default:
                throw error; // Unknown error, re-throw
        }
    }
}
```
</details>

**Beginner: Q3** - Can you use `await` without `async`?

<details>
<summary>Answer</summary>

No, `await` can only be used inside `async` functions (with one exception):

```javascript
// ❌ This will cause a SyntaxError
function regularFunction() {
    const result = await fetch('/api/data'); // SyntaxError
    return result;
}

// ✅ Correct usage
async function asyncFunction() {
    const result = await fetch('/api/data'); // Works
    return result;
}

// ❌ Cannot use await in global scope (older environments)
// const data = await fetch('/api/data'); // SyntaxError

// ✅ Wrap in async IIFE
(async () => {
    const data = await fetch('/api/data');
    console.log(data);
})();

// ✅ Or use in async function
async function main() {
    const data = await fetch('/api/data');
    console.log(data);
}
main();

// Exception: Top-level await (ES2022 in modules)
// ✅ Works in ES modules with top-level await support
// main.js (with "type": "module" in package.json)
const data = await fetch('/api/data'); // Works in modern environments

// Class methods can be async
class DataManager {
    async fetchData() {
        return await fetch('/api/data'); // Works
    }
    
    regularMethod() {
        // return await fetch('/api/data'); // SyntaxError
        return fetch('/api/data'); // Must return Promise directly
    }
}

// Arrow functions can be async
const asyncArrow = async () => {
    return await somePromise(); // Works
};

// Callback functions can be async
array.map(async (item) => {
    return await processItem(item); // Works, but be careful!
});

// Note: Be careful with async callbacks
// This creates an array of Promises, not resolved values
const results = array.map(async (item) => await processItem(item));
// To wait for all: await Promise.all(results)

// Event handlers can be async
button.addEventListener('click', async (event) => {
    const data = await fetch('/api/data'); // Works
    console.log(data);
});
```
</details>

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

<details>
<summary>Answer</summary>

Output:
```
3
1
4
2
```

Explanation:

```javascript
async function test() {
    console.log('1');        // Executes immediately
    await Promise.resolve(); // Pauses here, goes to event loop
    console.log('2');        // Executes later (microtask)
}

console.log('3');    // 1. First (synchronous)
test();              // 2. Calls test(), logs '1', then pauses at await
console.log('4');    // 3. Third (synchronous, continues after test() pauses)
                     // 4. '2' logs later when microtask executes

// Step-by-step execution:
// 1. console.log('3') - synchronous, executes immediately
// 2. test() is called:
//    - console.log('1') executes immediately
//    - await Promise.resolve() pauses function, returns control
// 3. console.log('4') - synchronous, executes immediately
// 4. Event loop processes microtasks:
//    - Promise.resolve() completes
//    - Execution resumes in test() after await
//    - console.log('2') executes

// Compare with non-async version:
function testSync() {
    console.log('1');
    // No await, so continues immediately
    console.log('2');
}

console.log('3');
testSync();      // Logs '1' then '2' immediately
console.log('4');
// Output would be: 3, 1, 2, 4

// The await creates an asynchronous boundary
// Even though Promise.resolve() resolves immediately,
// the await still schedules the continuation as a microtask
```
</details>

**Intermediate: Q2** - How do you handle multiple async operations concurrently with async/await?

<details>
<summary>Answer</summary>

Use `Promise.all()`, `Promise.allSettled()`, or `Promise.race()` with async/await:

```javascript
// Sequential execution (slow)
async function sequential() {
    const user = await fetchUser(1);        // Wait 1 second
    const posts = await fetchPosts(1);      // Wait another 1 second
    const comments = await fetchComments(1); // Wait another 1 second
    // Total: ~3 seconds
    return { user, posts, comments };
}

// Concurrent execution (fast)
async function concurrent() {
    // Start all operations simultaneously
    const userPromise = fetchUser(1);
    const postsPromise = fetchPosts(1);
    const commentsPromise = fetchComments(1);
    
    // Wait for all to complete
    const [user, posts, comments] = await Promise.all([
        userPromise,
        postsPromise,
        commentsPromise
    ]);
    // Total: ~1 second (fastest operation)
    return { user, posts, comments };
}

// Using Promise.all with async/await
async function fetchAllData() {
    try {
        const results = await Promise.all([
            fetch('/api/users').then(r => r.json()),
            fetch('/api/posts').then(r => r.json()),
            fetch('/api/comments').then(r => r.json())
        ]);
        
        const [users, posts, comments] = results;
        return { users, posts, comments };
    } catch (error) {
        // If any request fails, this catch block runs
        console.error('One or more requests failed:', error);
        throw error;
    }
}

// Promise.allSettled - doesn't fail if one operation fails
async function fetchAllDataSafely() {
    const results = await Promise.allSettled([
        fetchUser(1),
        fetchPosts(1),
        fetchComments(1)
    ]);
    
    // Check results individually
    const user = results[0].status === 'fulfilled' ? results[0].value : null;
    const posts = results[1].status === 'fulfilled' ? results[1].value : [];
    const comments = results[2].status === 'fulfilled' ? results[2].value : [];
    
    return { user, posts, comments };
}

// Promise.race - returns first completed (resolved or rejected)
async function fetchWithTimeout() {
    try {
        const result = await Promise.race([
            fetchUser(1),
            new Promise((_, reject) => 
                setTimeout(() => reject(new Error('Timeout')), 5000)
            )
        ]);
        return result;
    } catch (error) {
        if (error.message === 'Timeout') {
            console.error('Request timed out');
        }
        throw error;
    }
}

// Mixed approach - some sequential, some concurrent
async function optimizedFetch(userId) {
    // First fetch user data
    const user = await fetchUser(userId);
    
    // Then fetch user-specific data concurrently
    const [posts, profile, notifications] = await Promise.all([
        fetchUserPosts(user.id),
        fetchUserProfile(user.id),
        fetchUserNotifications(user.id)
    ]);
    
    return { user, posts, profile, notifications };
}

// Batching with concurrency limit
async function processBatch(items, batchSize = 3) {
    const results = [];
    
    for (let i = 0; i < items.length; i += batchSize) {
        const batch = items.slice(i, i + batchSize);
        const batchResults = await Promise.all(
            batch.map(item => processItem(item))
        );
        results.push(...batchResults);
    }
    
    return results;
}

// Error handling with concurrent operations
async function robustConcurrentFetch() {
    const operations = [
        fetchUser(1).catch(err => ({ error: 'User fetch failed', details: err })),
        fetchPosts(1).catch(err => ({ error: 'Posts fetch failed', details: err })),
        fetchComments(1).catch(err => ({ error: 'Comments fetch failed', details: err }))
    ];
    
    const results = await Promise.all(operations);
    
    // Filter out errors and successful results
    const successful = results.filter(result => !result.error);
    const failed = results.filter(result => result.error);
    
    return { successful, failed };
}
```
</details>

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
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

<details>
<summary>Answer</summary>

Use `=` to assign default values to parameters:

```javascript
// Basic default parameters
function greet(name = 'World', greeting = 'Hello') {
    return `${greeting}, ${name}!`;
}

console.log(greet()); // "Hello, World!"
console.log(greet('Alice')); // "Hello, Alice!"
console.log(greet('Bob', 'Hi')); // "Hi, Bob!"

// Default values can be expressions
function createUser(name, role = 'user', id = Date.now()) {
    return { name, role, id };
}

// Default values can reference other parameters
function calculateArea(width, height = width) {
    return width * height; // Square if height not provided
}

console.log(calculateArea(5)); // 25 (5 * 5)
console.log(calculateArea(5, 3)); // 15 (5 * 3)

// Only undefined triggers default value
function test(value = 'default') {
    return value;
}

console.log(test()); // 'default'
console.log(test(undefined)); // 'default'
console.log(test(null)); // null (not default)
console.log(test(0)); // 0 (not default)
console.log(test('')); // '' (not default)
```
</details>

**Beginner: Q2** - What are rest parameters and how do you use them?

<details>
<summary>Answer</summary>

Rest parameters collect remaining arguments into an array using `...`:

```javascript
// Rest parameters collect remaining arguments
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
console.log(sum(5)); // 5
console.log(sum()); // 0

// Mix regular parameters with rest
function introduce(name, age, ...hobbies) {
    return `${name} is ${age} years old and likes ${hobbies.join(', ')}`;
}

console.log(introduce('Alice', 25, 'reading', 'coding', 'gaming'));
// "Alice is 25 years old and likes reading, coding, gaming"

// Rest parameter must be last
function invalid(first, ...middle, last) {} // SyntaxError

// Replacing arguments object
function oldWay() {
    const args = Array.from(arguments); // Convert arguments to array
    return args.slice(1); // Skip first argument
}

function newWay(first, ...rest) {
    return rest; // Already an array
}

// Arrow functions with rest parameters
const multiply = (...nums) => nums.reduce((a, b) => a * b, 1);
console.log(multiply(2, 3, 4)); // 24
```
</details>

**Beginner: Q3** - What is the spread operator and how is it used with arrays?

<details>
<summary>Answer</summary>

Spread operator `...` expands arrays into individual elements:

```javascript
// Spread array elements
const numbers = [1, 2, 3];
console.log(...numbers); // 1 2 3 (individual arguments)

// Copy arrays
const original = [1, 2, 3];
const copy = [...original]; // Shallow copy
copy.push(4);
console.log(original); // [1, 2, 3] (unchanged)
console.log(copy); // [1, 2, 3, 4]

// Merge arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4]

// Insert elements
const numbers = [2, 3, 4];
const withOne = [1, ...numbers]; // [1, 2, 3, 4]
const withFive = [...numbers, 5]; // [2, 3, 4, 5]
const between = [1, ...numbers, 5, 6]; // [1, 2, 3, 4, 5, 6]

// Function arguments
function add(a, b, c) {
    return a + b + c;
}

const values = [1, 2, 3];
console.log(add(...values)); // 6 (same as add(1, 2, 3))

// Math operations
const scores = [85, 92, 78, 96, 88];
console.log(Math.max(...scores)); // 96
console.log(Math.min(...scores)); // 78

// Convert string to array
const word = 'hello';
const letters = [...word]; // ['h', 'e', 'l', 'l', 'o']
```
</details>

**Intermediate: Q1** - What will be the output of this code?
```javascript
function test(a = 1, b = a + 1, c = b + a) {
    return [a, b, c];
}
console.log(test(5));
console.log(test());
```

<details>
<summary>Answer</summary>

Output:
```
[5, 6, 11]
[1, 2, 3]
```

Explanation:

```javascript
function test(a = 1, b = a + 1, c = b + a) {
    return [a, b, c];
}

// First call: test(5)
// a = 5 (provided)
// b = a + 1 = 5 + 1 = 6
// c = b + a = 6 + 5 = 11
console.log(test(5)); // [5, 6, 11]

// Second call: test()
// a = 1 (default)
// b = a + 1 = 1 + 1 = 2
// c = b + a = 2 + 1 = 3
console.log(test()); // [1, 2, 3]

// Default parameters can reference previous parameters
// They are evaluated left to right
// Each parameter can use values of parameters to its left

// More examples:
function example(x = 0, y = x * 2, z = x + y) {
    return { x, y, z };
}

console.log(example(3)); // { x: 3, y: 6, z: 9 }
console.log(example()); // { x: 0, y: 0, z: 0 }

// Cannot reference parameters to the right
function invalid(a = b, b = 1) {} // ReferenceError: b is not defined
```
</details>

**Intermediate: Q2** - How can you use the spread operator to merge objects and arrays?

<details>
<summary>Answer</summary>

Spread operator works with both arrays and objects for merging:

```javascript
// Array merging
const fruits = ['apple', 'banana'];
const vegetables = ['carrot', 'broccoli'];
const food = [...fruits, ...vegetables]; // ['apple', 'banana', 'carrot', 'broccoli']

// Multiple arrays
const first = [1, 2];
const second = [3, 4];
const third = [5, 6];
const all = [...first, ...second, ...third]; // [1, 2, 3, 4, 5, 6]

// Object merging (ES2018+)
const person = { name: 'John', age: 30 };
const job = { title: 'Developer', company: 'Tech Corp' };
const employee = { ...person, ...job };
// { name: 'John', age: 30, title: 'Developer', company: 'Tech Corp' }

// Override properties (later values win)
const defaults = { theme: 'light', size: 'medium' };
const userPrefs = { theme: 'dark', notifications: true };
const settings = { ...defaults, ...userPrefs };
// { theme: 'dark', size: 'medium', notifications: true }

// Add new properties while spreading
const original = { a: 1, b: 2 };
const extended = { ...original, c: 3, a: 10 }; // Override 'a'
// { a: 10, b: 2, c: 3 }

// Conditional spreading
const includeExtra = true;
const config = {
    base: 'config',
    ...(includeExtra && { extra: 'data' }), // Only spread if condition is true
    ...(false && { skipped: 'value' }) // This won't be included
};

// Nested object merging (shallow only)
const user = {
    name: 'Alice',
    preferences: { theme: 'light', lang: 'en' }
};

const updates = {
    preferences: { theme: 'dark' } // This replaces entire preferences object
};

const merged = { ...user, ...updates };
// { name: 'Alice', preferences: { theme: 'dark' } } - lang is lost!

// Deep merge requires custom function or library
const deepMerged = {
    ...user,
    preferences: { ...user.preferences, ...updates.preferences }
};
// { name: 'Alice', preferences: { theme: 'dark', lang: 'en' } }
```
</details>

## Destructuring (arrays, objects)

**Beginner: Q1** - How do you destructure arrays and objects in JavaScript?

<details>
<summary>Answer</summary>

Destructuring extracts values from arrays and objects into variables:

```javascript
// Array destructuring
const numbers = [1, 2, 3, 4, 5];
const [first, second, third] = numbers;
console.log(first); // 1
console.log(second); // 2
console.log(third); // 3

// Skip elements
const [a, , c] = numbers; // Skip second element
console.log(a); // 1
console.log(c); // 3

// Object destructuring
const person = { name: 'Alice', age: 30, city: 'New York' };
const { name, age, city } = person;
console.log(name); // 'Alice'
console.log(age); // 30
console.log(city); // 'New York'

// Extract specific properties
const { name: personName, age: personAge } = person;
console.log(personName); // 'Alice'
console.log(personAge); // 30

// Function parameters
function greet({ name, age }) {
    return `Hello ${name}, you are ${age} years old`;
}

console.log(greet(person)); // "Hello Alice, you are 30 years old"

// Array in function parameters
function sum([a, b, c]) {
    return a + b + c;
}

console.log(sum([1, 2, 3])); // 6
```
</details>

**Beginner: Q2** - How do you assign default values during destructuring?

<details>
<summary>Answer</summary>

Use `=` to provide default values in case the property/element is undefined:

```javascript
// Array destructuring with defaults
const numbers = [1];
const [first = 0, second = 0, third = 0] = numbers;
console.log(first); // 1 (from array)
console.log(second); // 0 (default)
console.log(third); // 0 (default)

// Object destructuring with defaults
const person = { name: 'Alice' };
const { name = 'Unknown', age = 0, city = 'Unknown' } = person;
console.log(name); // 'Alice' (from object)
console.log(age); // 0 (default)
console.log(city); // 'Unknown' (default)

// Defaults only for undefined values
const data = { value: null, count: 0 };
const { value = 'default', count = 10, missing = 'fallback' } = data;
console.log(value); // null (not undefined, so no default)
console.log(count); // 0 (not undefined, so no default)
console.log(missing); // 'fallback' (undefined, so default used)

// Function parameters with defaults
function processUser({ name = 'Guest', role = 'user', active = true } = {}) {
    return { name, role, active };
}

console.log(processUser()); // { name: 'Guest', role: 'user', active: true }
console.log(processUser({ name: 'Alice' })); // { name: 'Alice', role: 'user', active: true }

// Combined with renaming
const config = { theme: 'dark' };
const { theme: selectedTheme = 'light', lang: language = 'en' } = config;
console.log(selectedTheme); // 'dark'
console.log(language); // 'en'
```
</details>

**Beginner: Q3** - How do you rename variables during object destructuring?

<details>
<summary>Answer</summary>

Use `:` to rename variables during object destructuring:

```javascript
// Basic renaming
const user = { name: 'Alice', age: 30 };
const { name: userName, age: userAge } = user;
console.log(userName); // 'Alice'
console.log(userAge); // 30
// console.log(name); // ReferenceError: name is not defined

// Avoid naming conflicts
const person = { name: 'Bob' };
const company = { name: 'Tech Corp' };

const { name: personName } = person;
const { name: companyName } = company;
console.log(personName); // 'Bob'
console.log(companyName); // 'Tech Corp'

// Renaming with defaults
const settings = { theme: 'dark' };
const { 
    theme: selectedTheme = 'light', 
    lang: selectedLang = 'en',
    notifications: enableNotifications = true 
} = settings;

console.log(selectedTheme); // 'dark'
console.log(selectedLang); // 'en'
console.log(enableNotifications); // true

// Function parameters with renaming
function displayUser({ name: fullName, age: years, email: contact }) {
    return `${fullName} (${years}) - Contact: ${contact}`;
}

const userData = { name: 'John Doe', age: 25, email: 'john@example.com' };
console.log(displayUser(userData)); // "John Doe (25) - Contact: john@example.com"

// Nested object renaming
const response = {
    data: {
        user: { id: 1, name: 'Alice' },
        meta: { count: 100 }
    }
};

const { data: { user: { name: displayName }, meta: { count: totalCount } } } = response;
console.log(displayName); // 'Alice'
console.log(totalCount); // 100
```
</details>

**Intermediate: Q1** - What will be the output of this code?
```javascript
const arr = [1, 2, 3, 4, 5];
const [first, , third, ...rest] = arr;
console.log(first, third, rest);
```

<details>
<summary>Answer</summary>

Output: `1 3 [4, 5]`

Explanation:

```javascript
const arr = [1, 2, 3, 4, 5];
const [first, , third, ...rest] = arr;

// Destructuring breakdown:
// first = arr[0] = 1
// (skip) = arr[1] = 2 (skipped with empty slot)
// third = arr[2] = 3
// ...rest = remaining elements = [4, 5]

console.log(first, third, rest); // 1 3 [4, 5]

// Step-by-step:
// Position 0: first gets value 1
// Position 1: empty (comma with no variable name)
// Position 2: third gets value 3
// Position 3+: rest parameter collects [4, 5]

// More examples of skipping elements:
const [a, , , d] = [10, 20, 30, 40]; // Skip 2nd and 3rd
console.log(a, d); // 10 40

const [x, , ...remaining] = [1, 2, 3, 4, 5];
console.log(x); // 1
console.log(remaining); // [3, 4, 5] (starts from position 2)

// Rest parameter must be last
// const [first, ...middle, last] = arr; // SyntaxError
```
</details>

**Intermediate: Q2** - How do you destructure nested objects and arrays?

<details>
<summary>Answer</summary>

Use nested patterns to destructure deeply nested structures:

```javascript
// Nested object destructuring
const user = {
    name: 'Alice',
    address: {
        street: '123 Main St',
        city: 'New York',
        coordinates: { lat: 40.7128, lng: -74.0060 }
    },
    hobbies: ['reading', 'coding', 'gaming']
};

// Extract nested object properties
const {
    name,
    address: { 
        city, 
        coordinates: { lat, lng } 
    },
    hobbies: [firstHobby, ...otherHobbies]
} = user;

console.log(name); // 'Alice'
console.log(city); // 'New York'
console.log(lat); // 40.7128
console.log(lng); // -74.0060
console.log(firstHobby); // 'reading'
console.log(otherHobbies); // ['coding', 'gaming']

// Nested arrays
const matrix = [[1, 2], [3, 4], [5, 6]];
const [[a, b], [c, d]] = matrix;
console.log(a, b, c, d); // 1 2 3 4

// Mixed nested destructuring
const response = {
    data: {
        users: [
            { id: 1, name: 'John', tags: ['admin', 'user'] },
            { id: 2, name: 'Jane', tags: ['user'] }
        ]
    }
};

const {
    data: {
        users: [
            { name: firstName, tags: [firstTag] },
            { name: secondName }
        ]
    }
} = response;

console.log(firstName); // 'John'
console.log(firstTag); // 'admin'
console.log(secondName); // 'Jane'

// With defaults for nested properties
const config = {
    server: {
        port: 3000
    }
};

const {
    server: { 
        port = 8080, 
        host = 'localhost' 
    } = {}
} = config;

console.log(port); // 3000
console.log(host); // 'localhost' (default)

// Function parameters with nested destructuring
function processOrder({ 
    customer: { name, email }, 
    items: [{ product, quantity }],
    shipping: { method = 'standard' } = {}
}) {
    return `Order for ${name} (${email}): ${quantity}x ${product}, shipped via ${method}`;
}

const order = {
    customer: { name: 'Bob', email: 'bob@example.com' },
    items: [{ product: 'Laptop', quantity: 1 }],
    shipping: { method: 'express' }
};

console.log(processOrder(order));
// "Order for Bob (bob@example.com): 1x Laptop, shipped via express"
```
</details>

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
import * as _ from './lodash.js';
const debouncedFn = _.debounce(myFunction, 300);
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

// 4. Plugin system
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

// Benefits:
// - Smaller initial bundle size
// - Faster page load
// - Load features on demand
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

// 1. Loading behavior
// CommonJS: Synchronous, runtime
const fs = require('fs'); // Loaded immediately

// ES6: Asynchronous, compile-time
import fs from 'fs'; // Analyzed before execution

// 2. Static vs Dynamic
// CommonJS: Dynamic (can be conditional)
if (condition) {
    const module = require('./module'); // Works
}

// ES6: Static (must be top-level)
// if (condition) {
//     import module from './module'; // SyntaxError
// }

// 3. Live bindings
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

// 4. Tree shaking
// CommonJS: No tree shaking
const utils = require('./utils'); // Entire module loaded

// ES6: Tree shaking possible
import { onlyThis } from './utils.js'; // Only used exports bundled
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

<details>
<summary>Answer</summary>

A Promise represents an eventual completion or failure of an asynchronous operation. It has three states:

```javascript
// Three states:
// 1. Pending - initial state, neither fulfilled nor rejected
// 2. Fulfilled (resolved) - operation completed successfully
// 3. Rejected - operation failed

// Creating a Promise
const promise = new Promise((resolve, reject) => {
    // Asynchronous operation
    setTimeout(() => {
        const success = Math.random() > 0.5;
        
        if (success) {
            resolve('Operation successful!'); // Fulfilled state
        } else {
            reject(new Error('Operation failed!')); // Rejected state
        }
    }, 1000);
});

// Promise states are immutable - once settled, they can't change
const resolvedPromise = Promise.resolve('Already resolved');
const rejectedPromise = Promise.reject(new Error('Already rejected'));

// Check promise state (not directly accessible, but can be observed)
promise
    .then(result => {
        console.log('Fulfilled:', result);
    })
    .catch(error => {
        console.log('Rejected:', error.message);
    });

// Promise state transitions:
// Pending → Fulfilled (via resolve())
// Pending → Rejected (via reject())
// Once settled (fulfilled or rejected), state cannot change
```
</details>

**Beginner: Q2** - How do you create a Promise and handle its result?

<details>
<summary>Answer</summary>

Create Promises using the Promise constructor and handle results with `.then()` and `.catch()`:

```javascript
// Creating a Promise
function fetchData() {
    return new Promise((resolve, reject) => {
        // Simulate async operation
        setTimeout(() => {
            const data = { id: 1, name: 'John' };
            const error = Math.random() > 0.8;
            
            if (error) {
                reject(new Error('Failed to fetch data'));
            } else {
                resolve(data);
            }
        }, 1000);
    });
}

// Handling Promise results
fetchData()
    .then(data => {
        console.log('Success:', data);
        return data.id; // Return value for next .then()
    })
    .then(id => {
        console.log('User ID:', id);
    })
    .catch(error => {
        console.error('Error:', error.message);
    })
    .finally(() => {
        console.log('Operation completed');
    });

// Alternative with async/await
async function handleData() {
    try {
        const data = await fetchData();
        console.log('Success:', data);
        return data;
    } catch (error) {
        console.error('Error:', error.message);
    } finally {
        console.log('Operation completed');
    }
}

// Creating resolved/rejected Promises directly
const immediateSuccess = Promise.resolve('Immediate success');
const immediateFailure = Promise.reject(new Error('Immediate failure'));

immediateSuccess.then(value => console.log(value));
immediateFailure.catch(error => console.log(error.message));

// Chaining Promises
function step1() {
    return Promise.resolve(1);
}

function step2(value) {
    return Promise.resolve(value * 2);
}

function step3(value) {
    return Promise.resolve(value + 10);
}

step1()
    .then(step2)
    .then(step3)
    .then(result => console.log('Final result:', result)); // 12
```
</details>

**Beginner: Q3** - What is the difference between `.then()` and `.catch()`?

<details>
<summary>Answer</summary>

`.then()` handles fulfilled promises, `.catch()` handles rejected promises:

```javascript
const promise = new Promise((resolve, reject) => {
    const success = Math.random() > 0.5;
    
    if (success) {
        resolve('Success!');
    } else {
        reject(new Error('Failed!'));
    }
});

// .then() - handles fulfilled promises
promise.then(value => {
    console.log('Fulfilled:', value);
});

// .catch() - handles rejected promises
promise.catch(error => {
    console.log('Rejected:', error.message);
});

// Combined usage
promise
    .then(value => {
        console.log('Success:', value);
        return value.toUpperCase();
    })
    .catch(error => {
        console.log('Error:', error.message);
        return 'DEFAULT VALUE'; // Recovery value
    });

// .then() can also handle both states
promise.then(
    value => console.log('Success:', value),    // onFulfilled
    error => console.log('Error:', error.message) // onRejected
);

// Error propagation
Promise.resolve(1)
    .then(x => {
        if (x === 1) throw new Error('Something went wrong');
        return x * 2;
    })
    .then(x => {
        console.log('This won\'t run');
        return x + 1;
    })
    .catch(error => {
        console.log('Caught:', error.message); // Catches the thrown error
        return 10; // Recovery value
    })
    .then(x => {
        console.log('Continues with:', x); // 10
    });

// Key differences:
// .then() - for success handling and transformation
// .catch() - for error handling and recovery
// .finally() - runs regardless of outcome
```
</details>

**Intermediate: Q1** - What is the difference between `Promise.all()`, `Promise.race()`, and `Promise.allSettled()`?

<details>
<summary>Answer</summary>

These methods handle multiple promises differently:

```javascript
const fast = new Promise(resolve => setTimeout(() => resolve('fast'), 100));
const slow = new Promise(resolve => setTimeout(() => resolve('slow'), 300));
const failing = new Promise((_, reject) => setTimeout(() => reject(new Error('failed')), 200));

// Promise.all() - waits for ALL to resolve, fails if ANY rejects
Promise.all([fast, slow])
    .then(results => console.log('All results:', results)) // ['fast', 'slow']
    .catch(error => console.log('Any failed:', error));

Promise.all([fast, failing, slow])
    .then(results => console.log('Won\'t run'))
    .catch(error => console.log('Failed because:', error.message)); // 'failed'

// Promise.race() - returns first settled (resolved OR rejected)
Promise.race([fast, slow])
    .then(result => console.log('First result:', result)) // 'fast'
    .catch(error => console.log('First error:', error));

Promise.race([failing, slow])
    .then(result => console.log('Won\'t run'))
    .catch(error => console.log('First to settle failed:', error.message)); // 'failed'

// Promise.allSettled() - waits for ALL to settle, never rejects
Promise.allSettled([fast, failing, slow])
    .then(results => {
        console.log('All settled:');
        results.forEach((result, index) => {
            if (result.status === 'fulfilled') {
                console.log(`${index}: Success -`, result.value);
            } else {
                console.log(`${index}: Failed -`, result.reason.message);
            }
        });
    });
// Output:
// 0: Success - fast
// 1: Failed - failed  
// 2: Success - slow

// Comparison table:
//                    | Promise.all | Promise.race | Promise.allSettled
// Waits for all      |     ✓      |      ✗       |        ✓
// Fails if one fails |     ✓      |      ✗       |        ✗
// Returns array      |     ✓      |      ✗       |        ✓
// Can reject         |     ✓      |      ✓       |        ✗

// Use cases:
// Promise.all: When you need ALL operations to succeed
// Promise.race: Timeout scenarios, fastest response
// Promise.allSettled: When you want results regardless of failures
```
</details>

**Intermediate: Q2** - What will be the output of this code?
```javascript
Promise.resolve(1)
    .then(x => x + 1)
    .then(x => { throw new Error('Error!'); })
    .then(x => x + 1)
    .catch(err => 5)
    .then(x => x + 1);
```

<details>
<summary>Answer</summary>

The final result will be a Promise that resolves to `6`.

Step-by-step execution:

```javascript
Promise.resolve(1)                           // Resolves to 1
    .then(x => x + 1)                       // 1 + 1 = 2
    .then(x => { throw new Error('Error!'); }) // Throws error, goes to catch
    .then(x => x + 1)                       // Skipped (previous threw error)
    .catch(err => 5)                        // Catches error, returns 5
    .then(x => x + 1);                      // 5 + 1 = 6

// Final result: Promise resolves to 6

// Execution flow:
// 1. Promise.resolve(1) → value: 1
// 2. .then(x => x + 1) → value: 2
// 3. .then(x => { throw new Error('Error!'); }) → error thrown
// 4. .then(x => x + 1) → SKIPPED (chain is in error state)
// 5. .catch(err => 5) → catches error, returns 5 (chain recovers)
// 6. .then(x => x + 1) → 5 + 1 = 6

// To see the result:
Promise.resolve(1)
    .then(x => x + 1)
    .then(x => { throw new Error('Error!'); })
    .then(x => x + 1)
    .catch(err => 5)
    .then(x => x + 1)
    .then(result => console.log('Final result:', result)); // 6

// Key points:
// - Error skips subsequent .then() handlers until .catch()
// - .catch() can return a value to recover the chain
// - After .catch() returns a value, chain continues normally
// - If .catch() threw an error, it would continue to next .catch()
```
</details>

## Event loop & call stack

**Beginner: Q1** - What is the call stack in JavaScript?

<details>
<summary>Answer</summary>

The call stack is a data structure that tracks function calls in JavaScript using LIFO (Last In, First Out):

```javascript
function first() {
    console.log('First function');
    second();
    console.log('First function ends');
}

function second() {
    console.log('Second function');
    third();
    console.log('Second function ends');
}

function third() {
    console.log('Third function');
}

first();

// Call stack execution:
// 1. first() pushed to stack
// 2. second() pushed to stack  
// 3. third() pushed to stack
// 4. third() completes, popped from stack
// 5. second() completes, popped from stack
// 6. first() completes, popped from stack

// Output:
// First function
// Second function  
// Third function
// Second function ends
// First function ends

// Stack visualization:
// Step 1: [first]
// Step 2: [first, second]
// Step 3: [first, second, third]
// Step 4: [first, second]
// Step 5: [first]
// Step 6: []

// Stack overflow example
function recursiveFunction() {
    recursiveFunction(); // No base case
}

// recursiveFunction(); // RangeError: Maximum call stack size exceeded

// Checking stack with console.trace()
function a() {
    console.trace('Stack trace from function a');
    b();
}

function b() {
    console.trace('Stack trace from function b');
}

a(); // Shows call stack in browser dev tools
```
</details>

**Beginner: Q2** - What is the event loop and how does it work?

<details>
<summary>Answer</summary>

The event loop manages asynchronous operations by coordinating the call stack, callback queue, and microtask queue:

```javascript
// Event loop phases:
// 1. Execute synchronous code (call stack)
// 2. Process microtasks (Promise callbacks)
// 3. Process macrotasks (setTimeout, setInterval)
// 4. Repeat

console.log('1'); // Synchronous - call stack

setTimeout(() => {
    console.log('2'); // Macrotask - callback queue
}, 0);

Promise.resolve().then(() => {
    console.log('3'); // Microtask - microtask queue
});

console.log('4'); // Synchronous - call stack

// Output: 1, 4, 3, 2

// Event loop workflow:
function eventLoopDemo() {
    console.log('Start');
    
    // Macrotask
    setTimeout(() => console.log('Timeout 1'), 0);
    
    // Microtask
    Promise.resolve().then(() => console.log('Promise 1'));
    
    // Another microtask
    Promise.resolve().then(() => {
        console.log('Promise 2');
        // Nested microtask
        Promise.resolve().then(() => console.log('Nested Promise'));
    });
    
    // Another macrotask
    setTimeout(() => console.log('Timeout 2'), 0);
    
    console.log('End');
}

eventLoopDemo();
// Output: Start, End, Promise 1, Promise 2, Nested Promise, Timeout 1, Timeout 2

// Visual representation:
// Call Stack: [eventLoopDemo] → [console.log] → empty
// Microtask Queue: [Promise 1] → [Promise 2] → [Nested Promise]
// Callback Queue: [Timeout 1] → [Timeout 2]
```
</details>

**Beginner: Q3** - What happens when the call stack is empty?

<details>
<summary>Answer</summary>

When the call stack is empty, the event loop processes queued tasks:

```javascript
// When call stack empties:
// 1. Process all microtasks first
// 2. Then process one macrotask
// 3. Check for more microtasks
// 4. Repeat

console.log('Start'); // Call stack: [console.log] → empty

setTimeout(() => {
    console.log('Macrotask 1'); // Added to callback queue
}, 0);

Promise.resolve().then(() => {
    console.log('Microtask 1'); // Added to microtask queue
    
    // Add another microtask
    Promise.resolve().then(() => {
        console.log('Microtask 2');
    });
});

setTimeout(() => {
    console.log('Macrotask 2'); // Added to callback queue
}, 0);

console.log('End'); // Call stack: [console.log] → empty

// Execution order:
// 1. 'Start' (call stack)
// 2. 'End' (call stack)
// 3. Call stack empty → process microtasks
// 4. 'Microtask 1' → 'Microtask 2' (all microtasks)
// 5. Process one macrotask: 'Macrotask 1'
// 6. Check microtasks (none), process next macrotask: 'Macrotask 2'

// Blocking the call stack
function blockingOperation() {
    const start = Date.now();
    while (Date.now() - start < 3000) {
        // Block for 3 seconds
    }
    console.log('Blocking done');
}

console.log('Before blocking');
setTimeout(() => console.log('This waits'), 0);
blockingOperation(); // Blocks everything
console.log('After blocking');

// setTimeout callback won't run until blockingOperation finishes
// Output: Before blocking, Blocking done, After blocking, This waits

// Non-blocking alternative
function nonBlockingOperation() {
    return new Promise(resolve => {
        setTimeout(() => {
            console.log('Non-blocking done');
            resolve();
        }, 0);
    });
}
```
</details>

**Intermediate: Q1** - What will be the output of this code?
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
console.log('3');
Promise.resolve().then(() => console.log('4'));
console.log('5');
```

<details>
<summary>Answer</summary>

Output:
```
1
3
5
4
2
```

Explanation:

```javascript
console.log('1');                              // 1. Call stack: synchronous
setTimeout(() => console.log('2'), 0);         // 2. Macrotask: added to callback queue
console.log('3');                              // 3. Call stack: synchronous
Promise.resolve().then(() => console.log('4')); // 4. Microtask: added to microtask queue
console.log('5');                              // 5. Call stack: synchronous

// Step-by-step execution:
// 1. console.log('1') → output: '1'
// 2. setTimeout schedules callback → callback queue: [() => console.log('2')]
// 3. console.log('3') → output: '3'
// 4. Promise.then schedules callback → microtask queue: [() => console.log('4')]
// 5. console.log('5') → output: '5'
// 6. Call stack empty → process microtasks first
// 7. Execute Promise callback → output: '4'
// 8. Microtask queue empty → process macrotasks
// 9. Execute setTimeout callback → output: '2'

// Event loop priority:
// Call Stack (synchronous) → Microtask Queue → Callback Queue (macrotasks)

// Even with setTimeout(0), it goes to callback queue
// Microtasks always have higher priority than macrotasks
```
</details>

**Intermediate: Q2** - Explain the relationship between the call stack, callback queue, and microtask queue.

<details>
<summary>Answer</summary>

These three components work together in the event loop with specific priority order:

```javascript
// 1. CALL STACK - Synchronous execution (highest priority)
function synchronousFunction() {
    console.log('Synchronous');
}

// 2. MICROTASK QUEUE - Promise callbacks, queueMicrotask (high priority)
Promise.resolve().then(() => console.log('Microtask'));
queueMicrotask(() => console.log('Explicit microtask'));

// 3. CALLBACK QUEUE (MACROTASK) - setTimeout, setInterval, DOM events (lower priority)
setTimeout(() => console.log('Macrotask'), 0);

// Execution priority:
// Call Stack → Microtask Queue → Callback Queue

// Detailed example:
function demonstrateQueues() {
    console.log('1: Synchronous start');
    
    // Macrotasks
    setTimeout(() => console.log('7: setTimeout 1'), 0);
    setTimeout(() => console.log('8: setTimeout 2'), 0);
    
    // Microtasks
    Promise.resolve().then(() => {
        console.log('4: Promise 1');
        Promise.resolve().then(() => console.log('5: Nested Promise'));
    });
    
    queueMicrotask(() => console.log('6: queueMicrotask'));
    
    Promise.resolve().then(() => console.log('9: Promise 2'));
    
    console.log('2: Synchronous middle');
    
    setTimeout(() => {
        console.log('10: setTimeout 3');
        Promise.resolve().then(() => console.log('11: Promise in setTimeout'));
    }, 0);
    
    console.log('3: Synchronous end');
}

demonstrateQueues();

// Output:
// 1: Synchronous start
// 2: Synchronous middle  
// 3: Synchronous end
// 4: Promise 1
// 5: Nested Promise
// 6: queueMicrotask
// 9: Promise 2
// 7: setTimeout 1
// 8: setTimeout 2
// 10: setTimeout 3
// 11: Promise in setTimeout

// Key relationships:
// 1. Call stack must be empty before processing queues
// 2. All microtasks processed before any macrotask
// 3. After each macrotask, check for microtasks again
// 4. Microtasks can spawn more microtasks (processed immediately)
// 5. Macrotasks are processed one at a time

// Queue types:
const examples = {
    callStack: ['function calls', 'synchronous code'],
    microtaskQueue: ['Promise.then()', 'queueMicrotask()', 'async/await'],
    macrotaskQueue: ['setTimeout()', 'setInterval()', 'DOM events', 'I/O operations']
};
```
</details>

## Callback functions

**Beginner: Q1** - What is a callback function? Give an example.

<details>
<summary>Answer</summary>

A callback function is a function passed as an argument to another function and executed later:

```javascript
// Basic callback example
function greet(name, callback) {
    console.log(`Hello ${name}`);
    callback();
}

function afterGreeting() {
    console.log('Nice to meet you!');
}

greet('Alice', afterGreeting);
// Output: 
// "Hello Alice"
// "Nice to meet you!"

// Array methods using callbacks
const numbers = [1, 2, 3, 4, 5];

// map() takes a callback
const doubled = numbers.map(function(num) {
    return num * 2;
});
console.log(doubled); // [2, 4, 6, 8, 10]

// filter() takes a callback
const evens = numbers.filter(function(num) {
    return num % 2 === 0;
});
console.log(evens); // [2, 4]

// Anonymous callback
numbers.forEach(function(num) {
    console.log(num);
});

// Arrow function callback
const squares = numbers.map(num => num * num);
console.log(squares); // [1, 4, 9, 16, 25]
```
</details>

**Beginner: Q2** - Why are callbacks used in asynchronous operations?

<details>
<summary>Answer</summary>

Callbacks allow code to continue executing while waiting for asynchronous operations to complete:

```javascript
// Synchronous operation (blocks execution)
function syncOperation() {
    for (let i = 0; i < 1000000000; i++) {
        // Blocking operation
    }
    return 'Done';
}

console.log('Before sync');
const result = syncOperation(); // Blocks here
console.log('After sync');

// Asynchronous operation with callback (non-blocking)
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: 'Alice' };
        callback(data); // Execute callback when data is ready
    }, 1000);
}

console.log('Before async');
fetchData(function(data) {
    console.log('Received data:', data);
});
console.log('After async'); // Executes immediately

// File reading example
const fs = require('fs');

// Asynchronous file read
fs.readFile('data.txt', 'utf8', function(err, data) {
    if (err) {
        console.error('Error reading file:', err);
    } else {
        console.log('File contents:', data);
    }
});

console.log('This runs while file is being read');

// Event handling with callbacks
button.addEventListener('click', function() {
    console.log('Button clicked!');
});

// AJAX request with callback
function makeRequest(url, callback) {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
            callback(JSON.parse(xhr.responseText));
        }
    };
    xhr.send();
}
```
</details>

**Beginner: Q3** - What is a callback passed to `setTimeout`?

<details>
<summary>Answer</summary>

The callback in `setTimeout` is the function executed after the specified delay:

```javascript
// Basic setTimeout with callback
setTimeout(function() {
    console.log('This runs after 2 seconds');
}, 2000);

console.log('This runs immediately');

// Arrow function callback
setTimeout(() => {
    console.log('Arrow function callback');
}, 1000);

// Named function as callback
function delayedFunction() {
    console.log('Named function executed');
}

setTimeout(delayedFunction, 500);

// Callback with parameters (using closure)
function greetUser(name) {
    setTimeout(function() {
        console.log(`Hello ${name}!`);
    }, 1000);
}

greetUser('Alice'); // "Hello Alice!" after 1 second

// Multiple timeouts
setTimeout(() => console.log('First'), 1000);
setTimeout(() => console.log('Second'), 2000);
setTimeout(() => console.log('Third'), 1500);
// Output order: "First" (1s), "Third" (1.5s), "Second" (2s)

// Clearing timeout
const timeoutId = setTimeout(() => {
    console.log('This might not run');
}, 3000);

// Cancel the timeout
clearTimeout(timeoutId);

// Common pattern: delayed execution
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

const debouncedLog = debounce(console.log, 300);
debouncedLog('Hello'); // Only the last call executes after 300ms
```
</details>

**Intermediate: Q1** - How do you handle errors in callback-based functions?

<details>
<summary>Answer</summary>

Use error-first callback pattern where the first parameter is an error object:

```javascript
// Error-first callback pattern (Node.js style)
function readFileAsync(filename, callback) {
    setTimeout(() => {
        if (filename === 'invalid.txt') {
            const error = new Error('File not found');
            callback(error, null); // Error first, data second
        } else {
            const data = 'File contents here';
            callback(null, data); // No error, data provided
        }
    }, 100);
}

// Handling the callback
readFileAsync('data.txt', function(err, data) {
    if (err) {
        console.error('Error occurred:', err.message);
        return;
    }
    console.log('File data:', data);
});

// Try-catch doesn't work with async callbacks
function asyncOperation(callback) {
    setTimeout(() => {
        throw new Error('Async error'); // This won't be caught
    }, 100);
}

try {
    asyncOperation(function() {
        console.log('Done');
    });
} catch (error) {
    console.log('This will NOT catch the error');
}

// Proper error handling in callbacks
function safeAsyncOperation(callback) {
    setTimeout(() => {
        try {
            // Potentially dangerous operation
            const result = riskyFunction();
            callback(null, result);
        } catch (error) {
            callback(error, null); // Pass error to callback
        }
    }, 100);
}

// Multiple operation error handling
function fetchUserData(userId, callback) {
    fetchUser(userId, function(err, user) {
        if (err) return callback(err);
        
        fetchUserPosts(user.id, function(err, posts) {
            if (err) return callback(err);
            
            callback(null, { user, posts });
        });
    });
}

// Usage with error handling
fetchUserData(123, function(err, data) {
    if (err) {
        console.error('Failed to fetch user data:', err.message);
        return;
    }
    console.log('Success:', data);
});
```
</details>

**Intermediate: Q2** - What are the problems with deeply nested callbacks?

<details>
<summary>Answer</summary>

Deeply nested callbacks create several problems including readability and error handling:

```javascript
// Callback hell example - pyramid of doom
function fetchUserProfile(userId) {
    fetchUser(userId, function(err, user) {
        if (err) {
            console.error('User fetch failed:', err);
            return;
        }
        
        fetchUserPosts(user.id, function(err, posts) {
            if (err) {
                console.error('Posts fetch failed:', err);
                return;
            }
            
            fetchPostComments(posts[0].id, function(err, comments) {
                if (err) {
                    console.error('Comments fetch failed:', err);
                    return;
                }
                
                fetchUserProfile(comments[0].authorId, function(err, commenter) {
                    if (err) {
                        console.error('Commenter fetch failed:', err);
                        return;
                    }
                    
                    // Finally use the data
                    displayProfile({
                        user: user,
                        posts: posts,
                        comments: comments,
                        commenter: commenter
                    });
                });
            });
        });
    });
}

// Problems with this approach:

// 1. Readability - "Pyramid of Doom"
// 2. Error handling repetition
// 3. Difficult to maintain and debug
// 4. Hard to test individual functions
// 5. Inversion of control

// Solutions:

// 1. Named functions
function handleUser(err, user) {
    if (err) return console.error('User error:', err);
    fetchUserPosts(user.id, handlePosts);
}

function handlePosts(err, posts) {
    if (err) return console.error('Posts error:', err);
    fetchPostComments(posts[0].id, handleComments);
}

function handleComments(err, comments) {
    if (err) return console.error('Comments error:', err);
    // Continue...
}

fetchUser(userId, handleUser);

// 2. Promises
fetchUser(userId)
    .then(user => fetchUserPosts(user.id))
    .then(posts => fetchPostComments(posts[0].id))
    .then(comments => fetchUserProfile(comments[0].authorId))
    .then(commenter => displayProfile({ commenter }))
    .catch(err => console.error('Error:', err));

// 3. Async/await
async function fetchUserProfileAsync(userId) {
    try {
        const user = await fetchUser(userId);
        const posts = await fetchUserPosts(user.id);
        const comments = await fetchPostComments(posts[0].id);
        const commenter = await fetchUserProfile(comments[0].authorId);
        
        displayProfile({ user, posts, comments, commenter });
    } catch (err) {
        console.error('Error:', err);
    }
}
```
</details>

## Callback hell

**Beginner: Q1** - What is callback hell?

<details>
<summary>Answer</summary>

Callback hell refers to deeply nested callbacks that make code hard to read and maintain:

```javascript
// Callback hell example
getData(function(a) {
    getMoreData(a, function(b) {
        getMoreData(b, function(c) {
            getMoreData(c, function(d) {
                getMoreData(d, function(e) {
                    // Finally do something with e
                    console.log(e);
                });
            });
        });
    });
});

// Real-world example
function orderFood() {
    checkMenu(function(menu) {
        selectItem(menu, function(item) {
            checkInventory(item, function(available) {
                if (available) {
                    placeOrder(item, function(order) {
                        processPayment(order, function(payment) {
                            if (payment.success) {
                                prepareFood(order, function(food) {
                                    deliverFood(food, function(delivered) {
                                        console.log('Food delivered!', delivered);
                                    });
                                });
                            }
                        });
                    });
                }
            });
        });
    });
}

// Characteristics of callback hell:
// 1. Pyramid-like structure (indentation grows)
// 2. Hard to read and follow
// 3. Difficult error handling
// 4. Poor maintainability
// 5. Testing becomes complex
```
</details>

**Beginner: Q2** - Why is callback hell considered a problem?

<details>
<summary>Answer</summary>

Callback hell creates multiple development and maintenance issues:

```javascript
// 1. Readability Problem
fetchUser(id, function(user) {
    fetchPosts(user.id, function(posts) {
        fetchComments(posts[0].id, function(comments) {
            // Hard to see the flow of logic
            displayData(user, posts, comments);
        });
    });
});

// 2. Error Handling Complexity
fetchUser(id, function(err, user) {
    if (err) return handleError(err);
    
    fetchPosts(user.id, function(err, posts) {
        if (err) return handleError(err);
        
        fetchComments(posts[0].id, function(err, comments) {
            if (err) return handleError(err);
            
            // Repetitive error handling
            displayData(user, posts, comments);
        });
    });
});

// 3. Debugging Difficulty
// Stack traces become confusing
// Hard to set breakpoints effectively
// Difficult to trace execution flow

// 4. Testing Challenges
// Hard to test individual pieces
// Mocking becomes complex
// Unit tests are difficult to write

// 5. Code Reusability
// Functions become tightly coupled
// Hard to extract reusable components
// Logic is spread across callbacks

// 6. Modification Issues
// Adding new steps requires restructuring
// Changing order of operations is complex
// Parallel execution becomes difficult

// Compare with Promise-based approach:
fetchUser(id)
    .then(user => fetchPosts(user.id))
    .then(posts => fetchComments(posts[0].id))
    .then(comments => displayData(comments))
    .catch(handleError); // Single error handler

// Or async/await:
try {
    const user = await fetchUser(id);
    const posts = await fetchPosts(user.id);
    const comments = await fetchComments(posts[0].id);
    displayData(user, posts, comments);
} catch (error) {
    handleError(error);
}
```
</details>

**Beginner: Q3** - Give an example of callback hell.

<details>
<summary>Answer</summary>

Here's a practical example showing progression from simple to callback hell:

```javascript
// Simple callback (no problem)
setTimeout(function() {
    console.log('Timer finished');
}, 1000);

// Nested callbacks (beginning of problem)
setTimeout(function() {
    console.log('First timer');
    setTimeout(function() {
        console.log('Second timer');
    }, 1000);
}, 1000);

// Callback hell example - File processing
function processFiles() {
    readFile('config.json', function(err, config) {
        if (err) throw err;
        
        readFile(config.inputFile, function(err, data) {
            if (err) throw err;
            
            processData(data, function(err, processed) {
                if (err) throw err;
                
                validateData(processed, function(err, valid) {
                    if (err) throw err;
                    
                    if (valid) {
                        saveFile(config.outputFile, processed, function(err) {
                            if (err) throw err;
                            
                            logActivity('Processing complete', function(err) {
                                if (err) throw err;
                                
                                sendNotification('File processed successfully', function(err) {
                                    if (err) throw err;
                                    console.log('All done!');
                                });
                            });
                        });
                    } else {
                        console.log('Data validation failed');
                    }
                });
            });
        });
    });
}

// E-commerce example
function purchaseItem(userId, itemId) {
    getUser(userId, function(err, user) {
        if (err) return handleError(err);
        
        getItem(itemId, function(err, item) {
            if (err) return handleError(err);
            
            checkInventory(item.id, function(err, available) {
                if (err) return handleError(err);
                
                if (available > 0) {
                    calculatePrice(item, user.discountLevel, function(err, price) {
                        if (err) return handleError(err);
                        
                        processPayment(user.paymentMethod, price, function(err, payment) {
                            if (err) return handleError(err);
                            
                            updateInventory(item.id, -1, function(err) {
                                if (err) return handleError(err);
                                
                                createOrder(user.id, item.id, payment.id, function(err, order) {
                                    if (err) return handleError(err);
                                    
                                    sendConfirmationEmail(user.email, order, function(err) {
                                        if (err) console.log('Email failed but order succeeded');
                                        console.log('Purchase complete!');
                                    });
                                });
                            });
                        });
                    });
                } else {
                    console.log('Item out of stock');
                }
            });
        });
    });
}

// This is callback hell because:
// 1. 8+ levels of nesting
// 2. Difficult to follow the logic
// 3. Error handling is repetitive
// 4. Hard to modify or extend
// 5. Testing individual steps is complex
```
</details>

**Intermediate: Q1** - How can Promises help solve callback hell?

<details>
<summary>Answer</summary>

Promises flatten nested callbacks into sequential chains:

```javascript
// Callback hell version
function fetchUserData(userId) {
    fetchUser(userId, function(err, user) {
        if (err) return console.error(err);
        
        fetchPosts(user.id, function(err, posts) {
            if (err) return console.error(err);
            
            fetchComments(posts[0].id, function(err, comments) {
                if (err) return console.error(err);
                
                console.log({ user, posts, comments });
            });
        });
    });
}

// Promise version
function fetchUserDataPromise(userId) {
    return fetchUser(userId)
        .then(user => {
            return fetchPosts(user.id).then(posts => ({ user, posts }));
        })
        .then(({ user, posts }) => {
            return fetchComments(posts[0].id).then(comments => ({ user, posts, comments }));
        })
        .then(data => {
            console.log(data);
        })
        .catch(err => {
            console.error(err);
        });
}

// Even better Promise version (flattened)
function fetchUserDataFlat(userId) {
    let userData = {};
    
    return fetchUser(userId)
        .then(user => {
            userData.user = user;
            return fetchPosts(user.id);
        })
        .then(posts => {
            userData.posts = posts;
            return fetchComments(posts[0].id);
        })
        .then(comments => {
            userData.comments = comments;
            return userData;
        })
        .catch(err => {
            console.error('Error:', err);
            throw err;
        });
}

// Using Promise.all for parallel operations
function fetchUserDataParallel(userId) {
    return fetchUser(userId)
        .then(user => {
            return Promise.all([
                Promise.resolve(user),
                fetchPosts(user.id),
                fetchUserProfile(user.id)
            ]);
        })
        .then(([user, posts, profile]) => {
            return { user, posts, profile };
        })
        .catch(err => {
            console.error('Error:', err);
        });
}

// Converting callback to Promise
function promisify(callbackFn) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            callbackFn(...args, (err, result) => {
                if (err) reject(err);
                else resolve(result);
            });
        });
    };
}

// Usage
const fetchUserPromise = promisify(fetchUser);
const fetchPostsPromise = promisify(fetchPosts);

fetchUserPromise(userId)
    .then(user => fetchPostsPromise(user.id))
    .then(posts => console.log(posts))
    .catch(console.error);

// Benefits of Promises:
// 1. Linear flow instead of nested callbacks
// 2. Single .catch() for error handling
// 3. Better readability and maintainability
// 4. Easier to test individual steps
// 5. Support for parallel execution with Promise.all
```
</details>

**Intermediate: Q2** - What are other strategies to avoid callback hell?

<details>
<summary>Answer</summary>

Several strategies can help avoid callback hell:

```javascript
// 1. Named Functions (Extract callbacks)
function handleUser(err, user) {
    if (err) return console.error('User error:', err);
    fetchPosts(user.id, handlePosts);
}

function handlePosts(err, posts) {
    if (err) return console.error('Posts error:', err);
    fetchComments(posts[0].id, handleComments);
}

function handleComments(err, comments) {
    if (err) return console.error('Comments error:', err);
    console.log('Success:', comments);
}

// Usage
fetchUser(userId, handleUser);

// 2. Modularization
const userModule = {
    fetch: function(id, callback) {
        // Implementation
        callback(null, { id, name: 'User' });
    },
    
    processData: function(user, callback) {
        // Process user data
        callback(null, processedUser);
    }
};

// 3. Control Flow Libraries (like async.js)
const async = require('async');

async.waterfall([
    function(callback) {
        fetchUser(userId, callback);
    },
    function(user, callback) {
        fetchPosts(user.id, callback);
    },
    function(posts, callback) {
        fetchComments(posts[0].id, callback);
    }
], function(err, result) {
    if (err) return console.error(err);
    console.log('Final result:', result);
});

// 4. Async/Await (ES2017)
async function fetchUserDataAsync(userId) {
    try {
        const user = await fetchUser(userId);
        const posts = await fetchPosts(user.id);
        const comments = await fetchComments(posts[0].id);
        
        return { user, posts, comments };
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}

// 5. Event Emitters
const EventEmitter = require('events');
class DataFetcher extends EventEmitter {
    fetchUserData(userId) {
        this.fetchUser(userId);
    }
    
    fetchUser(userId) {
        // Simulate async operation
        setTimeout(() => {
            this.emit('userFetched', { id: userId, name: 'User' });
        }, 100);
    }
    
    fetchPosts(userId) {
        setTimeout(() => {
            this.emit('postsFetched', [{ id: 1, title: 'Post' }]);
        }, 100);
    }
}

const fetcher = new DataFetcher();
fetcher.on('userFetched', user => fetcher.fetchPosts(user.id));
fetcher.on('postsFetched', posts => console.log('Posts:', posts));

// 6. State Machines
const states = {
    INITIAL: 'initial',
    FETCHING_USER: 'fetchingUser',
    FETCHING_POSTS: 'fetchingPosts',
    COMPLETE: 'complete',
    ERROR: 'error'
};

class DataFetcherState {
    constructor() {
        this.state = states.INITIAL;
        this.data = {};
    }
    
    async start(userId) {
        try {
            this.state = states.FETCHING_USER;
            this.data.user = await fetchUser(userId);
            
            this.state = states.FETCHING_POSTS;
            this.data.posts = await fetchPosts(this.data.user.id);
            
            this.state = states.COMPLETE;
            return this.data;
        } catch (error) {
            this.state = states.ERROR;
            throw error;
        }
    }
}

// 7. Reactive Programming (RxJS)
const { from } = require('rxjs');
const { mergeMap, map, catchError } = require('rxjs/operators');

from(fetchUser(userId))
    .pipe(
        mergeMap(user => fetchPosts(user.id)),
        map(posts => posts.filter(post => post.active)),
        catchError(err => {
            console.error('Error:', err);
            return [];
        })
    )
    .subscribe(posts => console.log('Posts:', posts));
```
</details>

## Promises & chaining

**Beginner: Q1** - How do you chain multiple Promises together?

<details>
<summary>Answer</summary>

Chain Promises using `.then()` methods, where each returns a new Promise:

```javascript
// Basic Promise chaining
fetchUser(1)
    .then(user => {
        console.log('User:', user);
        return fetchPosts(user.id);
    })
    .then(posts => {
        console.log('Posts:', posts);
        return fetchComments(posts[0].id);
    })
    .then(comments => {
        console.log('Comments:', comments);
    })
    .catch(error => {
        console.error('Error:', error);
    });

// Each .then() can return:
// 1. A value (wrapped in resolved Promise)
fetchData()
    .then(data => {
        return data.id; // Returns Promise.resolve(data.id)
    })
    .then(id => {
        console.log('ID:', id);
    });

// 2. A Promise
fetchUser(1)
    .then(user => {
        return fetchProfile(user.id); // Returns actual Promise
    })
    .then(profile => {
        console.log('Profile:', profile);
    });

// 3. Throw an error (rejected Promise)
fetchData()
    .then(data => {
        if (!data) {
            throw new Error('No data found');
        }
        return data;
    })
    .catch(error => {
        console.error('Caught:', error.message);
    });

// Practical example: Authentication flow
login(credentials)
    .then(authToken => {
        localStorage.setItem('token', authToken);
        return fetchUserProfile(authToken);
    })
    .then(profile => {
        updateUI(profile);
        return fetchUserPreferences(profile.id);
    })
    .then(preferences => {
        applyTheme(preferences.theme);
        console.log('Login complete');
    })
    .catch(error => {
        showErrorMessage(error.message);
        redirectToLogin();
    });
```
</details>

**Beginner: Q2** - What does each `.then()` return?

<details>
<summary>Answer</summary>

Each `.then()` returns a new Promise, enabling chaining:

```javascript
// .then() always returns a Promise
const promise1 = fetchData();
const promise2 = promise1.then(data => data.name);
const promise3 = promise2.then(name => name.toUpperCase());

console.log(promise1); // Promise (original)
console.log(promise2); // Promise (new one)
console.log(promise3); // Promise (another new one)

// What .then() returns depends on the handler's return value:

// 1. Return a value → Promise.resolve(value)
Promise.resolve(5)
    .then(x => x * 2)     // Returns Promise.resolve(10)
    .then(x => x + 3)     // Returns Promise.resolve(13)
    .then(console.log);   // Logs: 13

// 2. Return a Promise → That exact Promise
Promise.resolve(1)
    .then(x => {
        return Promise.resolve(x * 2); // Returns this Promise
    })
    .then(x => {
        console.log(x); // 2
    });

// 3. Throw an error → Promise.reject(error)
Promise.resolve(1)
    .then(x => {
        throw new Error('Something went wrong');
    })
    .then(x => {
        console.log('This will not run');
    })
    .catch(error => {
        console.log('Caught:', error.message);
    });

// 4. No return statement → Promise.resolve(undefined)
Promise.resolve('hello')
    .then(message => {
        console.log(message); // 'hello'
        // No return statement
    })
    .then(result => {
        console.log(result); // undefined
    });

// Chain behavior examples
fetchUser(1)
    .then(user => {
        console.log('Got user:', user.name);
        return user.id; // Promise.resolve(user.id)
    })
    .then(userId => {
        console.log('User ID:', userId);
        return fetchPosts(userId); // Returns actual Promise
    })
    .then(posts => {
        console.log('Got posts:', posts.length);
        // No return - returns Promise.resolve(undefined)
    })
    .then(result => {
        console.log('Result:', result); // undefined
    });
```
</details>

**Beginner: Q3** - How do you handle errors in a Promise chain?

<details>
<summary>Answer</summary>

Use `.catch()` to handle errors at any point in the Promise chain:

```javascript
// Basic error handling
fetchUser(1)
    .then(user => fetchPosts(user.id))
    .then(posts => processPosts(posts))
    .catch(error => {
        console.error('Error occurred:', error.message);
    });

// Multiple catch blocks
fetchData()
    .then(data => {
        if (!data) throw new Error('No data');
        return processData(data);
    })
    .catch(error => {
        console.error('Processing error:', error);
        return defaultData; // Recovery
    })
    .then(result => {
        console.log('Final result:', result);
    })
    .catch(error => {
        console.error('Final error:', error);
    });

// Specific error handling
fetchUserData(userId)
    .then(user => {
        if (user.status === 'inactive') {
            throw new Error('User inactive');
        }
        return fetchUserPosts(user.id);
    })
    .then(posts => {
        if (posts.length === 0) {
            throw new Error('No posts found');
        }
        return posts;
    })
    .catch(error => {
        if (error.message === 'User inactive') {
            redirectToActivation();
        } else if (error.message === 'No posts found') {
            showEmptyState();
        } else {
            showGenericError(error);
        }
    });

// Error propagation
step1()
    .then(result1 => {
        if (result1.error) {
            throw new Error('Step 1 failed');
        }
        return step2(result1);
    })
    .then(result2 => {
        return step3(result2); // If this fails, goes to catch
    })
    .then(result3 => {
        console.log('Success:', result3);
    })
    .catch(error => {
        // Catches errors from any step
        console.error('Pipeline failed:', error.message);
    });

// Finally block equivalent
fetchData()
    .then(data => processData(data))
    .catch(error => {
        console.error('Error:', error);
        throw error; // Re-throw to continue error chain
    })
    .finally(() => {
        hideLoadingSpinner(); // Always runs
        console.log('Cleanup complete');
    });

// Error recovery patterns
fetchFromPrimary()
    .catch(error => {
        console.warn('Primary failed, trying backup');
        return fetchFromBackup();
    })
    .catch(error => {
        console.warn('Backup failed, using cache');
        return fetchFromCache();
    })
    .catch(error => {
        console.error('All sources failed');
        return defaultValue;
    })
    .then(data => {
        console.log('Got data:', data);
    });
```
</details>

**Intermediate: Q1** - What happens if you don't return anything from a `.then()` handler?

<details>
<summary>Answer</summary>

When no return statement is used, `.then()` returns `Promise.resolve(undefined)`:

```javascript
// No return statement
Promise.resolve('hello')
    .then(message => {
        console.log(message); // 'hello'
        // No return statement
    })
    .then(result => {
        console.log(result); // undefined
        console.log(typeof result); // 'undefined'
    });

// Comparison with explicit return
Promise.resolve('hello')
    .then(message => {
        console.log(message); // 'hello'
        return message.toUpperCase(); // Explicit return
    })
    .then(result => {
        console.log(result); // 'HELLO'
    });

// Common mistake: Forgetting to return Promise
function fetchUserData(id) {
    return getUser(id)
        .then(user => {
            getUserPosts(user.id); // Missing return!
            // This returns Promise.resolve(undefined)
        })
        .then(posts => {
            console.log(posts); // undefined, not the posts!
        });
}

// Correct version
function fetchUserData(id) {
    return getUser(id)
        .then(user => {
            return getUserPosts(user.id); // Proper return
        })
        .then(posts => {
            console.log(posts); // Actual posts array
        });
}

// Side effects without return
fetchData()
    .then(data => {
        // Side effects are fine without return
        updateUI(data);
        logAnalytics(data);
        cacheData(data);
        // No return needed if next .then() doesn't need data
    })
    .then(() => {
        // This handler receives undefined
        console.log('UI updated, analytics logged');
    });

// When to return vs not return
Promise.resolve(5)
    .then(x => {
        console.log('Processing:', x);
        // Don't return if you want to break the chain
        saveToDatabase(x);
    })
    .then(result => {
        console.log(result); // undefined
        // Can't use the original value here
    });

// Better approach: Return for chaining
Promise.resolve(5)
    .then(x => {
        console.log('Processing:', x);
        saveToDatabase(x);
        return x; // Keep value in chain
    })
    .then(x => {
        console.log('Original value still available:', x);
        return x * 2;
    })
    .then(doubled => {
        console.log('Doubled:', doubled);
    });
```
</details>

**Intermediate: Q2** - How do you convert callback-based functions to Promises?

<details>
<summary>Answer</summary>

Wrap callback functions in Promise constructor or use utility functions:

```javascript
// Manual conversion using Promise constructor
function readFilePromise(filename) {
    return new Promise((resolve, reject) => {
        fs.readFile(filename, 'utf8', (err, data) => {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
}

// Usage
readFilePromise('data.txt')
    .then(content => console.log(content))
    .catch(error => console.error(error));

// Generic promisify utility
function promisify(callbackFn) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            callbackFn(...args, (err, result) => {
                if (err) {
                    reject(err);
                } else {
                    resolve(result);
                }
            });
        });
    };
}

// Convert multiple functions
const fs = require('fs');
const readFileAsync = promisify(fs.readFile);
const writeFileAsync = promisify(fs.writeFile);

// Node.js built-in promisify
const { promisify } = require('util');
const readFileAsync = promisify(fs.readFile);

// Custom callback conversion examples
function timeoutCallback(delay, callback) {
    setTimeout(() => {
        callback(null, `Completed after ${delay}ms`);
    }, delay);
}

function timeoutPromise(delay) {
    return new Promise((resolve, reject) => {
        timeoutCallback(delay, (err, result) => {
            if (err) reject(err);
            else resolve(result);
        });
    });
}

// Database query conversion
function queryCallback(sql, params, callback) {
    // Simulate database query
    setTimeout(() => {
        if (sql.includes('SELECT')) {
            callback(null, [{ id: 1, name: 'John' }]);
        } else {
            callback(new Error('Invalid query'));
        }
    }, 100);
}

function queryPromise(sql, params) {
    return new Promise((resolve, reject) => {
        queryCallback(sql, params, (err, results) => {
            if (err) {
                reject(err);
            } else {
                resolve(results);
            }
        });
    });
}

// Usage
queryPromise('SELECT * FROM users', [])
    .then(users => console.log('Users:', users))
    .catch(error => console.error('Query failed:', error));

// Converting APIs with different callback signatures
function apiCallCallback(endpoint, options, successCallback, errorCallback) {
    setTimeout(() => {
        if (endpoint.startsWith('/api/')) {
            successCallback({ data: 'API response' });
        } else {
            errorCallback(new Error('Invalid endpoint'));
        }
    }, 100);
}

function apiCallPromise(endpoint, options) {
    return new Promise((resolve, reject) => {
        apiCallCallback(
            endpoint,
            options,
            (response) => resolve(response), // Success callback
            (error) => reject(error)        // Error callback
        );
    });
}

// Class method conversion
class DatabaseService {
    queryCallback(sql, callback) {
        setTimeout(() => {
            callback(null, { rows: [], count: 0 });
        }, 100);
    }
    
    queryPromise(sql) {
        return new Promise((resolve, reject) => {
            this.queryCallback(sql, (err, result) => {
                if (err) reject(err);
                else resolve(result);
            });
        });
    }
}

const db = new DatabaseService();
db.queryPromise('SELECT * FROM users')
    .then(result => console.log('Query result:', result))
    .catch(error => console.error('Query error:', error));
```
</details>

## Microtasks vs macrotasks

**Beginner: Q1** - What are microtasks and macrotasks?

<details>
<summary>Answer</summary>

Microtasks and macrotasks are different queues for asynchronous operations in the event loop:

```javascript
// MACROTASKS (Task Queue)
// - setTimeout, setInterval
// - setImmediate (Node.js)
// - I/O operations
// - UI events (clicks, etc.)

setTimeout(() => {
    console.log('Macrotask: setTimeout');
}, 0);

// MICROTASKS (Microtask Queue) 
// - Promise.then/catch/finally
// - queueMicrotask()
// - MutationObserver
// - process.nextTick (Node.js - highest priority)

Promise.resolve().then(() => {
    console.log('Microtask: Promise');
});

queueMicrotask(() => {
    console.log('Microtask: queueMicrotask');
});

// Event Loop Processing Order:
// 1. Execute synchronous code
// 2. Process ALL microtasks
// 3. Process ONE macrotask
// 4. Process ALL microtasks again
// 5. Repeat steps 3-4

// Visualization:
// Call Stack: [main execution]
// Microtask Queue: [Promise callbacks, queueMicrotask]
// Macrotask Queue: [setTimeout, setInterval, I/O]

console.log('sync start');

setTimeout(() => console.log('macro 1'), 0);
Promise.resolve().then(() => console.log('micro 1'));
setTimeout(() => console.log('macro 2'), 0);
Promise.resolve().then(() => console.log('micro 2'));

console.log('sync end');

// Output:
// sync start
// sync end
// micro 1
// micro 2
// macro 1
// macro 2
```
</details>

**Beginner: Q2** - Which has higher priority: microtasks or macrotasks?

<details>
<summary>Answer</summary>

Microtasks have higher priority than macrotasks and must be fully emptied before processing macrotasks:

```javascript
// Microtasks are processed BEFORE macrotasks
console.log('1: sync start');

// Macrotask
setTimeout(() => {
    console.log('4: macrotask');
}, 0);

// Microtask  
Promise.resolve().then(() => {
    console.log('3: microtask');
});

console.log('2: sync end');

// Output order:
// 1: sync start
// 2: sync end  
// 3: microtask (processed first)
// 4: macrotask (processed after all microtasks)

// Event loop priority:
// 1. Call Stack (synchronous code)
// 2. Microtask Queue (ALL microtasks)
// 3. Macrotask Queue (ONE macrotask)
// 4. Back to step 2

// Microtasks can add more microtasks
Promise.resolve().then(() => {
    console.log('Micro 1');
    
    // Adding another microtask
    Promise.resolve().then(() => {
        console.log('Micro 2 (nested)');
    });
    
    queueMicrotask(() => {
        console.log('Micro 3 (queued)');
    });
});

setTimeout(() => {
    console.log('Macro 1 (waits for all microtasks)');
}, 0);

// Output:
// Micro 1
// Micro 2 (nested)
// Micro 3 (queued)
// Macro 1 (waits for all microtasks)

// Potential infinite microtask loop (blocks macrotasks)
function infiniteMicrotasks() {
    Promise.resolve().then(() => {
        console.log('Microtask running...');
        infiniteMicrotasks(); // Keeps adding microtasks
    });
}

// This would block setTimeout from running:
// infiniteMicrotasks();
// setTimeout(() => console.log('This may never run'), 0);

// In Node.js: process.nextTick has even higher priority
process.nextTick(() => console.log('nextTick'));
Promise.resolve().then(() => console.log('Promise'));
setTimeout(() => console.log('setTimeout'), 0);

// Node.js output:
// nextTick (highest priority)
// Promise 
// setTimeout
```
</details>

**Beginner: Q3** - Give examples of microtasks and macrotasks.

<details>
<summary>Answer</summary>

Here are common examples of microtasks and macrotasks:

```javascript
// MICROTASKS (higher priority)

// 1. Promise callbacks
Promise.resolve('data').then(data => {
    console.log('Promise.then:', data);
});

Promise.reject('error').catch(err => {
    console.log('Promise.catch:', err);
});

// 2. queueMicrotask()
queueMicrotask(() => {
    console.log('queueMicrotask callback');
});

// 3. async/await (uses Promise microtasks)
async function asyncFunction() {
    console.log('async function start');
    await Promise.resolve();
    console.log('after await (microtask)');
}
asyncFunction();

// 4. MutationObserver (browser)
if (typeof MutationObserver !== 'undefined') {
    const observer = new MutationObserver(() => {
        console.log('DOM mutation detected');
    });
}

// 5. process.nextTick (Node.js only)
if (typeof process !== 'undefined') {
    process.nextTick(() => {
        console.log('process.nextTick');
    });
}

// MACROTASKS (lower priority)

// 1. setTimeout & setInterval
setTimeout(() => {
    console.log('setTimeout 0ms');
}, 0);

setInterval(() => {
    console.log('setInterval');
}, 1000);

// 2. setImmediate (Node.js)
if (typeof setImmediate !== 'undefined') {
    setImmediate(() => {
        console.log('setImmediate');
    });
}

// 3. I/O operations
if (typeof require !== 'undefined') {
    const fs = require('fs');
    fs.readFile('example.txt', (err, data) => {
        console.log('File read complete');
    });
}

// 4. User events (browser)
if (typeof document !== 'undefined') {
    document.addEventListener('click', () => {
        console.log('User clicked');
    });
}

// 5. Network requests
fetch('/api/data')
    .then(response => {
        console.log('Fetch response (Promise - microtask)');
        return response.json();
    })
    .then(data => {
        console.log('JSON parsed (Promise - microtask)');
    });

// Mixed example showing execution order
console.log('=== Execution Order Demo ===');

// Macrotasks
setTimeout(() => console.log('setTimeout 1'), 0);
setTimeout(() => console.log('setTimeout 2'), 0);

// Microtasks
Promise.resolve().then(() => console.log('Promise 1'));
Promise.resolve().then(() => console.log('Promise 2'));

queueMicrotask(() => console.log('queueMicrotask 1'));
queueMicrotask(() => console.log('queueMicrotask 2'));

console.log('Synchronous code');

// Expected output:
// === Execution Order Demo ===
// Synchronous code
// Promise 1
// Promise 2
// queueMicrotask 1
// queueMicrotask 2
// setTimeout 1
// setTimeout 2
```
</details>

**Intermediate: Q1** - What will be the execution order of this code?
```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
console.log('end');
```

<details>
<summary>Answer</summary>

Output:
```
start
end
promise
timeout
```

Explanation:

```javascript
console.log('start');                                    // 1. Synchronous
setTimeout(() => console.log('timeout'), 0);             // 2. Macrotask queued
Promise.resolve().then(() => console.log('promise'));    // 3. Microtask queued  
console.log('end');                                      // 4. Synchronous

// Execution steps:

// Step 1: Execute synchronous code
console.log('start');  // Output: "start"
console.log('end');    // Output: "end"

// Step 2: Call stack is empty, check microtask queue
// Microtask queue: [Promise.then callback]
// Execute: console.log('promise')  // Output: "promise"

// Step 3: Microtask queue is empty, check macrotask queue
// Macrotask queue: [setTimeout callback]  
// Execute: console.log('timeout')  // Output: "timeout"

// Event loop phases:
// 1. Synchronous execution (call stack)
// 2. Microtask queue (ALL microtasks)
// 3. Macrotask queue (ONE macrotask)
// 4. Repeat from step 2

// More complex example:
console.log('=== Complex Example ===');

console.log('sync 1');

setTimeout(() => {
    console.log('macro 1');
    Promise.resolve().then(() => console.log('micro inside macro'));
}, 0);

Promise.resolve().then(() => {
    console.log('micro 1');
    setTimeout(() => console.log('macro inside micro'), 0);
});

setTimeout(() => console.log('macro 2'), 0);
Promise.resolve().then(() => console.log('micro 2'));

console.log('sync 2');

// Output:
// === Complex Example ===
// sync 1
// sync 2
// micro 1
// micro 2
// macro 1
// micro inside macro
// macro 2
// macro inside micro

// The key principle:
// Microtasks ALWAYS run before the next macrotask
// Even if microtasks are created by macrotasks
```
</details>

**Intermediate: Q2** - How does the event loop process microtasks and macrotasks?

## setTimeout, setInterval

**Beginner: Q1** - What is the difference between `setTimeout` and `setInterval`?

<details>
<summary>Answer</summary>

`setTimeout` executes code once after a delay, while `setInterval` executes code repeatedly at regular intervals:

```javascript
// setTimeout - executes once after delay
setTimeout(() => {
    console.log('This runs once after 2 seconds');
}, 2000);

// setInterval - executes repeatedly every interval
const intervalId = setInterval(() => {
    console.log('This runs every 1 second');
}, 1000);

// Comparison example
console.log('Start');

// This will run once after 1 second
setTimeout(() => {
    console.log('setTimeout: 1 second passed');
}, 1000);

// This will run every 500ms
let counter = 0;
const interval = setInterval(() => {
    counter++;
    console.log(`setInterval: ${counter * 500}ms passed`);
    
    if (counter === 5) {
        clearInterval(interval); // Stop after 5 executions
    }
}, 500);

// Output:
// Start
// setInterval: 500ms passed
// setTimeout: 1 second passed
// setInterval: 1000ms passed
// setInterval: 1500ms passed
// setInterval: 2000ms passed
// setInterval: 2500ms passed

// setTimeout with recursive pattern (alternative to setInterval)
function recursiveTimeout() {
    console.log('Recursive timeout');
    setTimeout(recursiveTimeout, 1000); // Schedule next execution
}
recursiveTimeout();

// Key differences:
// 1. setTimeout: One-time execution
// 2. setInterval: Repeated execution
// 3. setTimeout is more precise for timing
// 4. setInterval can accumulate delays
// 5. setTimeout allows dynamic delays
```
</details>

**Beginner: Q2** - How do you cancel a timeout or interval?

<details>
<summary>Answer</summary>

Use `clearTimeout()` for timeouts and `clearInterval()` for intervals:

```javascript
// Canceling setTimeout
const timeoutId = setTimeout(() => {
    console.log('This will not execute');
}, 5000);

// Cancel the timeout before it executes
clearTimeout(timeoutId);
console.log('Timeout canceled');

// Canceling setInterval
const intervalId = setInterval(() => {
    console.log('This will not execute');
}, 1000);

// Cancel the interval immediately
clearInterval(intervalId);
console.log('Interval canceled');

// Practical example: Auto-save with cancellation
class AutoSave {
    constructor() {
        this.saveTimeout = null;
        this.data = '';
    }
    
    scheduleAutoSave() {
        // Cancel any existing timeout
        if (this.saveTimeout) {
            clearTimeout(this.saveTimeout);
        }
        
        // Schedule new save after 2 seconds of inactivity
        this.saveTimeout = setTimeout(() => {
            this.save();
        }, 2000);
    }
    
    save() {
        console.log('Auto-saving data:', this.data);
        this.saveTimeout = null; // Clear reference
    }
    
    updateData(newData) {
        this.data = newData;
        this.scheduleAutoSave(); // Reschedule save
    }
}

const autoSave = new AutoSave();
autoSave.updateData('Hello'); // Schedules save
autoSave.updateData('Hello World'); // Cancels previous, schedules new
// Only saves after 2 seconds of no updates

// Cleanup pattern for intervals
class Timer {
    constructor() {
        this.interval = null;
        this.seconds = 0;
    }
    
    start() {
        if (this.interval) return; // Already running
        
        this.interval = setInterval(() => {
            this.seconds++;
            console.log(`Timer: ${this.seconds} seconds`);
        }, 1000);
    }
    
    stop() {
        if (this.interval) {
            clearInterval(this.interval);
            this.interval = null;
        }
    }
    
    reset() {
        this.stop();
        this.seconds = 0;
    }
}

const timer = new Timer();
timer.start(); // Start counting
setTimeout(() => timer.stop(), 5000); // Stop after 5 seconds

// Component cleanup (React example)
function useTimer() {
    const [seconds, setSeconds] = useState(0);
    
    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds(s => s + 1);
        }, 1000);
        
        // Cleanup on unmount
        return () => clearInterval(interval);
    }, []);
    
    return seconds;
}

// Important notes:
// 1. Always store the returned ID
// 2. Clear timers to prevent memory leaks
// 3. Check if ID exists before clearing
// 4. Set reference to null after clearing
// 5. Clear in cleanup functions (componentWillUnmount, useEffect return)
```
</details>

**Beginner: Q3** - What is the minimum delay you can set with `setTimeout`?

<details>
<summary>Answer</summary>

The minimum delay is browser-dependent but generally 1ms, though actual execution is usually 4ms or more:

```javascript
// Minimum delay test
console.log('Start:', Date.now());

setTimeout(() => {
    console.log('1ms timeout:', Date.now());
}, 1);

setTimeout(() => {
    console.log('0ms timeout:', Date.now());
}, 0);

// Even with 0ms, there's still a minimum delay
setTimeout(() => {
    console.log('Immediate timeout:', Date.now());
});

// Browser implementation details:
// - Chrome/Firefox: minimum ~4ms delay
// - Safari: minimum ~4ms delay  
// - Node.js: minimum ~1ms delay
// - HTML5 spec suggests 4ms minimum

// Nested setTimeout delays increase
function nestedTimeouts(depth) {
    if (depth > 0) {
        const start = performance.now();
        setTimeout(() => {
            const end = performance.now();
            console.log(`Depth ${5-depth}: ${end - start}ms delay`);
            nestedTimeouts(depth - 1);
        }, 0);
    }
}

nestedTimeouts(5);
// Output shows increasing delays:
// Depth 0: ~4ms
// Depth 1: ~4ms  
// Depth 2: ~4ms
// Depth 3: ~4ms
// Depth 4: ~4ms

// Better alternatives for immediate execution:
// 1. Promise.resolve().then()
Promise.resolve().then(() => {
    console.log('Microtask - executes before setTimeout');
});

setTimeout(() => {
    console.log('Macrotask - executes after microtasks');
}, 0);

// 2. queueMicrotask()
queueMicrotask(() => {
    console.log('queueMicrotask - also executes before setTimeout');
});

// 3. MessageChannel (for immediate async execution)
const channel = new MessageChannel();
channel.port2.onmessage = () => {
    console.log('MessageChannel - faster than setTimeout');
};

channel.port1.postMessage('execute');

// Performance comparison
function measureDelay(method, iterations = 1000) {
    return new Promise(resolve => {
        let count = 0;
        const start = performance.now();
        
        function execute() {
            count++;
            if (count >= iterations) {
                const end = performance.now();
                resolve((end - start) / iterations);
            } else {
                method(execute);
            }
        }
        
        execute();
    });
}

// Test different methods
async function compareDelays() {
    const setTimeoutDelay = await measureDelay(fn => setTimeout(fn, 0));
    const promiseDelay = await measureDelay(fn => Promise.resolve().then(fn));
    const microtaskDelay = await measureDelay(fn => queueMicrotask(fn));
    
    console.log('Average delays:');
    console.log(`setTimeout(0): ${setTimeoutDelay.toFixed(2)}ms`);
    console.log(`Promise.resolve(): ${promiseDelay.toFixed(2)}ms`);
    console.log(`queueMicrotask(): ${microtaskDelay.toFixed(2)}ms`);
}

compareDelays();

// Practical implications:
// 1. Don't rely on exact timing for setTimeout(0)
// 2. Use microtasks for immediate async execution
// 3. Browser throttling affects background tabs
// 4. Mobile devices may have different minimums
// 5. High-frequency animations should use requestAnimationFrame

// Special case: Background tabs
// Many browsers throttle setTimeout in background tabs to ~1000ms minimum
let isVisible = !document.hidden;

document.addEventListener('visibilitychange', () => {
    isVisible = !document.hidden;
    console.log('Tab is', isVisible ? 'visible' : 'hidden');
});

function scheduleWork(callback) {
    if (isVisible) {
        setTimeout(callback, 0); // Normal delay when visible
    } else {
        // Use different strategy for background tabs
        Promise.resolve().then(callback);
    }
}
```
</details>

**Intermediate: Q1** - What are the potential issues with `setInterval`? How would you fix them?

<details>
<summary>Answer</summary>

`setInterval` can accumulate delays and overlap executions. Use recursive `setTimeout` for better control:

```javascript
// Problem 1: Execution overlap
console.log('Problem: setInterval with slow operations');

let counter = 0;
const problemInterval = setInterval(() => {
    console.log('Start execution:', ++counter);
    
    // Simulate slow operation (200ms)
    const start = Date.now();
    while (Date.now() - start < 200) {
        // Blocking operation
    }
    
    console.log('End execution:', counter);
}, 100); // Interval shorter than execution time!

// This causes overlapping executions and performance issues
setTimeout(() => clearInterval(problemInterval), 1000);

// Solution 1: Recursive setTimeout
function recursiveTimeout(callback, delay) {
    function execute() {
        callback();
        setTimeout(execute, delay); // Schedule next after current finishes
    }
    return execute;
}

console.log('\nSolution: Recursive setTimeout');
let counter2 = 0;
const solution1 = recursiveTimeout(() => {
    console.log('Safe execution:', ++counter2);
    
    // Same slow operation
    const start = Date.now();
    while (Date.now() - start < 200) {}
    
    console.log('Safe end:', counter2);
}, 100);

solution1();

// Problem 2: Accumulating delays
async function simulateNetworkCall() {
    // Random delay between 50-300ms
    const delay = Math.random() * 250 + 50;
    return new Promise(resolve => setTimeout(resolve, delay));
}

// setInterval doesn't wait for async operations
console.log('\nProblem: setInterval with async operations');
let asyncCounter = 0;
const asyncProblem = setInterval(async () => {
    console.log('Async start:', ++asyncCounter);
    await simulateNetworkCall();
    console.log('Async end:', asyncCounter);
}, 100);

setTimeout(() => clearInterval(asyncProblem), 1000);

// Solution 2: Async-aware recursive setTimeout
async function asyncRecursiveTimeout(callback, delay) {
    async function execute() {
        await callback();
        setTimeout(execute, delay);
    }
    return execute;
}

console.log('\nSolution: Async recursive setTimeout');
let asyncCounter2 = 0;
const asyncSolution = await asyncRecursiveTimeout(async () => {
    console.log('Async safe start:', ++asyncCounter2);
    await simulateNetworkCall();
    console.log('Async safe end:', asyncCounter2);
}, 100);

asyncSolution();

// Problem 3: Browser throttling in background
// setInterval gets throttled to minimum 1000ms in background tabs

// Solution 3: Page Visibility API awareness
class SmartTimer {
    constructor(callback, interval) {
        this.callback = callback;
        this.interval = interval;
        this.timeoutId = null;
        this.running = false;
        this.lastExecution = 0;
        
        // Handle visibility changes
        document.addEventListener('visibilitychange', () => {
            this.handleVisibilityChange();
        });
    }
    
    start() {
        if (this.running) return;
        this.running = true;
        this.scheduleNext();
    }
    
    stop() {
        if (this.timeoutId) {
            clearTimeout(this.timeoutId);
            this.timeoutId = null;
        }
        this.running = false;
    }
    
    scheduleNext() {
        if (!this.running) return;
        
        this.timeoutId = setTimeout(() => {
            this.execute();
        }, this.interval);
    }
    
    async execute() {
        this.lastExecution = Date.now();
        
        try {
            await this.callback();
        } catch (error) {
            console.error('Timer callback error:', error);
        }
        
        this.scheduleNext();
    }
    
    handleVisibilityChange() {
        if (document.hidden) {
            console.log('Tab hidden - timer continues but may be throttled');
        } else {
            console.log('Tab visible - checking if catch-up needed');
            
            // If we've been hidden for a while, might need catch-up
            const timeSinceLastExecution = Date.now() - this.lastExecution;
            if (timeSinceLastExecution > this.interval * 2) {
                console.log('Executing catch-up');
                this.execute(); // Immediate execution
            }
        }
    }
}

// Usage
const smartTimer = new SmartTimer(async () => {
    console.log('Smart timer execution at', new Date().toLocaleTimeString());
    await simulateNetworkCall();
}, 2000);

smartTimer.start();

// Problem 4: Memory leaks with closures
function createLeakyTimer() {
    const largeData = new Array(1000000).fill('data'); // Large object
    
    setInterval(() => {
        // This closure keeps largeData in memory
        console.log('Timer with large data:', largeData.length);
    }, 1000);
}

// Solution 4: Proper cleanup and avoiding large closures
class ProperTimer {
    constructor() {
        this.intervalId = null;
        this.data = null;
    }
    
    start(dataSize) {
        // Don't store large data in closure scope
        this.data = new Array(dataSize).fill('data');
        
        this.intervalId = setInterval(() => {
            // Access data through 'this' instead of closure
            if (this.data) {
                console.log('Timer with proper data access:', this.data.length);
            }
        }, 1000);
    }
    
    stop() {
        if (this.intervalId) {
            clearInterval(this.intervalId);
            this.intervalId = null;
        }
        // Explicitly clean up large data
        this.data = null;
    }
}

const properTimer = new ProperTimer();
properTimer.start(1000000);

// Clean up after 5 seconds
setTimeout(() => {
    properTimer.stop();
}, 5000);

// Best practices summary:
// 1. Use recursive setTimeout for sequential operations
// 2. Handle async operations properly
// 3. Consider page visibility changes
// 4. Clean up references to prevent memory leaks
// 5. Use requestAnimationFrame for animations
// 6. Implement error handling in timer callbacks
// 7. Make timers pausable/resumable when needed
```
</details>

**Intermediate: Q2** - What will be the output of this code?

```javascript
for (var i = 1; i <= 3; i++) {
    setTimeout(() => console.log(i), 100);
}
```

<details>
<summary>Answer</summary>

The output will be `4 4 4` because of how `var` and closures work with the event loop:

```javascript
// Original code
for (var i = 1; i <= 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Output: 4 4 4

// Explanation of what happens:
// 1. Loop runs synchronously, i goes from 1 to 4
// 2. Three setTimeout callbacks are scheduled
// 3. When callbacks execute, loop has finished and i = 4
// 4. All three callbacks reference the same variable i

console.log('--- Step by step execution ---');

// Step 1: Loop executes (synchronous)
console.log('Loop starting...');
for (var i = 1; i <= 3; i++) {
    console.log('Scheduling timeout for i =', i);
    setTimeout(() => {
        console.log('Timeout executing, i is now:', i);
    }, 100);
}
console.log('Loop finished, final i =', i); // i = 4

// Solutions to get the expected output (1, 2, 3):

// Solution 1: Use let instead of var (block scoping)
console.log('\n--- Solution 1: let ---');
for (let i = 1; i <= 3; i++) {
    setTimeout(() => console.log('let solution:', i), 200);
}
// Output: 1 2 3

// Solution 2: Use IIFE to capture the value
console.log('\n--- Solution 2: IIFE ---');
for (var i = 1; i <= 3; i++) {
    (function(capturedI) {
        setTimeout(() => console.log('IIFE solution:', capturedI), 300);
    })(i);
}

// Solution 3: Use bind to capture the value
console.log('\n--- Solution 3: bind ---');
for (var i = 1; i <= 3; i++) {
    setTimeout(function(capturedI) {
        console.log('bind solution:', capturedI);
    }.bind(null, i), 400);
}

// Solution 4: Use closure with function
console.log('\n--- Solution 4: closure function ---');
function createTimeout(value) {
    return () => console.log('closure solution:', value);
}

for (var i = 1; i <= 3; i++) {
    setTimeout(createTimeout(i), 500);
}

// Solution 5: Use arrow function with default parameter
console.log('\n--- Solution 5: default parameter ---');
for (var i = 1; i <= 3; i++) {
    setTimeout((capturedI = i) => {
        console.log('default param solution:', capturedI);
    }, 600);
}

// Visual representation of the problem:
console.log('\n--- Visual representation ---');

// What we think happens (wrong):
// setTimeout(() => console.log(1), 100)  // i = 1
// setTimeout(() => console.log(2), 100)  // i = 2
// setTimeout(() => console.log(3), 100)  // i = 3

// What actually happens:
// Loop: i = 1, schedule timeout
// Loop: i = 2, schedule timeout  
// Loop: i = 3, schedule timeout
// Loop: i = 4, loop exits
// All timeouts execute and see i = 4

// Demonstrating the difference with execution order:
console.log('\n--- Execution order demonstration ---');

console.log('1. Synchronous code starts');

for (var i = 1; i <= 3; i++) {
    console.log(`2. Loop iteration ${i}, scheduling timeout`);
    setTimeout(() => {
        console.log(`4. Timeout executes, i = ${i}`);
    }, 100);
}

console.log('3. Loop finished, all timeouts scheduled');

// Advanced example showing the timing:
console.log('\n--- Advanced timing example ---');

const startTime = Date.now();

for (var i = 1; i <= 3; i++) {
    const currentI = i;
    setTimeout(() => {
        const elapsed = Date.now() - startTime;
        console.log(`Time: ${elapsed}ms, var i = ${i}, captured i = ${currentI}`);
    }, 100 * i); // Different delays
}

// This will show:
// Time: ~100ms, var i = 4, captured i = 1
// Time: ~200ms, var i = 4, captured i = 2  
// Time: ~300ms, var i = 4, captured i = 3

// Real-world scenario where this matters:
console.log('\n--- Real-world scenario ---');

const buttons = ['Button 1', 'Button 2', 'Button 3'];

// Wrong way (common bug):
for (var i = 0; i < buttons.length; i++) {
    setTimeout(() => {
        console.log(`Clicked ${buttons[i]}`); // All show "undefined"
    }, 1000);
}

// Correct way:
for (let i = 0; i < buttons.length; i++) {
    setTimeout(() => {
        console.log(`Correctly clicked ${buttons[i]}`);
    }, 1100);
}

// Key takeaways:
// 1. var has function scope, not block scope
// 2. Closures capture variables by reference, not value
// 3. setTimeout is asynchronous - executes after loop completes
// 4. Use let for block scoping in loops
// 5. Use IIFE or function parameters to capture values with var
```
</details>

## Web APIs

**Beginner: Q1** - What are Web APIs in the context of JavaScript?

<details>
<summary>Answer</summary>

Web APIs are browser-provided interfaces that extend JavaScript's capabilities beyond the core language:

```javascript
// Core JavaScript (ECMAScript) - available everywhere
const arr = [1, 2, 3];
const obj = { name: 'John' };
function greet() { return 'Hello'; }

// Web APIs - browser-specific extensions
// These are NOT part of JavaScript language itself

// 1. DOM API - Document Object Model
const element = document.getElementById('myElement');
element.addEventListener('click', () => {
    element.style.color = 'red';
});

// 2. Fetch API - Network requests  
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data));

// 3. Geolocation API
navigator.geolocation.getCurrentPosition(position => {
    console.log('Latitude:', position.coords.latitude);
    console.log('Longitude:', position.coords.longitude);
});

// 4. Local Storage API
localStorage.setItem('user', JSON.stringify({ name: 'John' }));
const user = JSON.parse(localStorage.getItem('user'));

// 5. Canvas API
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext('2d');
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 100, 100);

// 6. Web Audio API
const audioContext = new AudioContext();
const oscillator = audioContext.createOscillator();
oscillator.frequency.setValueAtTime(440, audioContext.currentTime);

// 7. WebRTC API
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
    .then(stream => {
        const video = document.querySelector('video');
        video.srcObject = stream;
    });

// Key characteristics of Web APIs:
// 1. Browser-specific implementations
// 2. Extend JavaScript capabilities  
// 3. Provide access to browser features
// 4. Often asynchronous operations
// 5. Follow web standards (W3C, WHATWG)

// Web API vs JavaScript example:
// JavaScript (core language):
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2); // Pure JavaScript

// Web API (browser extension):
const notification = new Notification('Hello!', { // Web API
    body: 'This is a browser notification',
    icon: '/icon.png'
});

// Feature detection (checking if API exists):
function checkWebAPISupport() {
    const features = {
        localStorage: typeof Storage !== 'undefined',
        geolocation: 'geolocation' in navigator,
        notifications: 'Notification' in window,
        webGL: !!document.createElement('canvas').getContext('webgl'),
        serviceWorker: 'serviceWorker' in navigator,
        webAssembly: typeof WebAssembly === 'object'
    };
    
    console.log('Supported Web APIs:', features);
    return features;
}

checkWebAPISupport();

// Polyfill example (adding missing Web API):
if (!window.fetch) {
    // Polyfill fetch for older browsers
    window.fetch = function(url, options) {
        return new Promise((resolve, reject) => {
            const xhr = new XMLHttpRequest();
            xhr.open(options?.method || 'GET', url);
            
            xhr.onload = () => {
                resolve({
                    json: () => Promise.resolve(JSON.parse(xhr.responseText)),
                    text: () => Promise.resolve(xhr.responseText)
                });
            };
            
            xhr.onerror = reject;
            xhr.send(options?.body);
        });
    };
}

// Browser-specific Web APIs:
// Chrome: Chrome Extension APIs, File System Access API
// Firefox: Firefox WebExtensions API  
// Safari: Safari App Extensions API
// Edge: Edge Extension APIs

// Web APIs provide:
// - DOM manipulation
// - Network communication
// - Hardware access (camera, microphone, GPS)
// - Storage mechanisms
// - Graphics and multimedia
// - Background processing
// - Push notifications
// - Real-time communication
```
</details>

**Beginner: Q2** - Name some common Web APIs available in browsers.

<details>
<summary>Answer</summary>

Here are commonly used Web APIs with practical examples:

```javascript
// 1. DOM API - Document manipulation
const element = document.createElement('div');
element.textContent = 'Hello World';
document.body.appendChild(element);

document.addEventListener('DOMContentLoaded', () => {
    console.log('DOM fully loaded');
});

// 2. Fetch API - HTTP requests
async function getData() {
    try {
        const response = await fetch('/api/users');
        const users = await response.json();
        return users;
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

// 3. Geolocation API - Location services
function getLocation() {
    if ('geolocation' in navigator) {
        navigator.geolocation.getCurrentPosition(
            position => {
                console.log('Lat:', position.coords.latitude);
                console.log('Lng:', position.coords.longitude);
            },
            error => console.error('Location error:', error)
        );
    }
}

// 4. Storage APIs - Data persistence
// LocalStorage - persistent across browser sessions
localStorage.setItem('theme', 'dark');
const theme = localStorage.getItem('theme');

// SessionStorage - cleared when tab closes
sessionStorage.setItem('tempData', 'value');

// IndexedDB - More powerful database
const request = indexedDB.open('MyDB', 1);
request.onsuccess = event => {
    const db = event.target.result;
    console.log('Database opened successfully');
};

// 5. Canvas API - 2D graphics
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Draw a circle
ctx.beginPath();
ctx.arc(100, 100, 50, 0, 2 * Math.PI);
ctx.fillStyle = 'blue';
ctx.fill();

// 6. Web Audio API - Audio processing
const audioContext = new (window.AudioContext || window.webkitAudioContext)();

async function playTone(frequency, duration) {
    const oscillator = audioContext.createOscillator();
    const gainNode = audioContext.createGain();
    
    oscillator.connect(gainNode);
    gainNode.connect(audioContext.destination);
    
    oscillator.frequency.value = frequency;
    oscillator.start();
    
    setTimeout(() => oscillator.stop(), duration);
}

// 7. MediaDevices API - Camera/Microphone access  
async function getCamera() {
    try {
        const stream = await navigator.mediaDevices.getUserMedia({
            video: true,
            audio: true
        });
        
        const video = document.querySelector('video');
        video.srcObject = stream;
    } catch (error) {
        console.error('Camera access denied:', error);
    }
}

// 8. Notifications API - Browser notifications
function showNotification() {
    if ('Notification' in window) {
        Notification.requestPermission().then(permission => {
            if (permission === 'granted') {
                new Notification('Hello!', {
                    body: 'This is a notification',
                    icon: '/icon.png',
                    tag: 'greeting'
                });
            }
        });
    }
}

// 9. WebSockets API - Real-time communication
const socket = new WebSocket('wss://api.example.com/ws');

socket.onopen = () => {
    console.log('WebSocket connected');
    socket.send('Hello Server!');
};

socket.onmessage = event => {
    console.log('Received:', event.data);
};

// 10. Service Worker API - Background processing
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js')
        .then(registration => {
            console.log('SW registered:', registration);
        })
        .catch(error => {
            console.log('SW registration failed:', error);
        });
}

// 11. Intersection Observer API - Lazy loading
const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            // Element is visible, load image
            entry.target.src = entry.target.dataset.src;
            observer.unobserve(entry.target);
        }
    });
});

document.querySelectorAll('img[data-src]').forEach(img => {
    observer.observe(img);
});

// 12. Clipboard API - Copy/paste operations
async function copyToClipboard(text) {
    try {
        await navigator.clipboard.writeText(text);
        console.log('Text copied to clipboard');
    } catch (error) {
        console.error('Failed to copy:', error);
    }
}

async function readFromClipboard() {
    try {
        const text = await navigator.clipboard.readText();
        console.log('Clipboard content:', text);
    } catch (error) {
        console.error('Failed to read clipboard:', error);
    }
}

// 13. Page Visibility API - Tab focus detection
document.addEventListener('visibilitychange', () => {
    if (document.hidden) {
        console.log('Tab is hidden');
        // Pause videos, stop animations
    } else {
        console.log('Tab is visible');
        // Resume activities
    }
});

// 14. Battery API (deprecated but still in some browsers)
if ('getBattery' in navigator) {
    navigator.getBattery().then(battery => {
        console.log('Battery level:', battery.level * 100 + '%');
        console.log('Charging:', battery.charging);
    });
}

// 15. Vibration API - Mobile device vibration
if ('vibrate' in navigator) {
    // Vibrate for 200ms
    navigator.vibrate(200);
    
    // Vibration pattern: vibrate, pause, vibrate
    navigator.vibrate([100, 50, 100]);
}

// 16. Performance API - Performance monitoring
const observer2 = new PerformanceObserver(list => {
    list.getEntries().forEach(entry => {
        console.log('Performance entry:', entry);
    });
});

observer2.observe({ entryTypes: ['navigation', 'resource'] });

// 17. Resize Observer API - Element size changes
const resizeObserver = new ResizeObserver(entries => {
    entries.forEach(entry => {
        console.log('Element resized:', entry.contentRect);
    });
});

resizeObserver.observe(document.body);

// Categories of Web APIs:
const webAPICategories = {
    DOM: ['Document', 'Element', 'Event'],
    Graphics: ['Canvas', 'WebGL', 'SVG'],
    Media: ['WebAudio', 'WebRTC', 'MediaDevices'],
    Storage: ['localStorage', 'sessionStorage', 'IndexedDB'],
    Network: ['Fetch', 'WebSocket', 'EventSource'],
    Device: ['Geolocation', 'DeviceOrientation', 'Battery'],
    Background: ['ServiceWorker', 'WebWorker', 'SharedWorker'],
    Security: ['Crypto', 'Permissions', 'CredentialManagement']
};

console.log('Web API Categories:', webAPICategories);
```
</details>

**Beginner: Q3** - Are Web APIs part of the JavaScript language itself?

<details>
<summary>Answer</summary>

No, Web APIs are not part of the JavaScript language itself. They are browser-provided extensions:

```javascript
// JavaScript (ECMAScript) - Core language features
// These work in ANY JavaScript environment (browser, Node.js, etc.)

// Variables and data types
const name = 'John';
const age = 30;
const isActive = true;

// Functions
function greet(person) {
    return `Hello, ${person}!`;
}

// Objects and arrays
const user = { name, age };
const numbers = [1, 2, 3, 4, 5];

// Control structures
if (age >= 18) {
    console.log('Adult');
}

// Array methods
const doubled = numbers.map(n => n * 2);

// Promises (added to ECMAScript 2015)
const promise = new Promise((resolve, reject) => {
    resolve('Success');
});

// Classes (ECMAScript 2015)
class Person {
    constructor(name) {
        this.name = name;
    }
}

// ============================================

// Web APIs - Browser-specific extensions
// These DON'T work in Node.js or other non-browser environments

// DOM API (Browser only)
// document.getElementById('myElement'); // ❌ Error in Node.js

// Fetch API (Browser only, but polyfills exist for Node.js)
// fetch('https://api.example.com'); // ❌ Error in Node.js (without polyfill)

// Browser APIs that don't exist in Node.js:
// window.alert('Hello'); // ❌ Error in Node.js
// localStorage.setItem('key', 'value'); // ❌ Error in Node.js
// navigator.geolocation.getCurrentPosition(); // ❌ Error in Node.js

// Demonstrating the difference:
function demonstrateEnvironmentDifferences() {
    console.log('=== Core JavaScript (works everywhere) ===');
    
    // These work in browser, Node.js, React Native, etc.
    const data = [1, 2, 3];
    const result = data.reduce((sum, num) => sum + num, 0);
    console.log('Sum:', result); // ✅ Works everywhere
    
    console.log('=== Web APIs (browser only) ===');
    
    // Check if we're in a browser environment
    if (typeof window !== 'undefined') {
        console.log('Running in browser - Web APIs available');
        
        // These only work in browsers
        console.log('User Agent:', navigator.userAgent);
        console.log('URL:', window.location.href);
        
        // DOM manipulation
        const div = document.createElement('div');
        console.log('Created div element:', div);
        
    } else {
        console.log('Running in Node.js - Web APIs not available');
        
        // Node.js specific APIs (not Web APIs)
        const fs = require('fs'); // ❌ This doesn't work in browsers
        const path = require('path');
    }
}

// Environment detection
function detectEnvironment() {
    const environment = {
        isBrowser: typeof window !== 'undefined',
        isNode: typeof process !== 'undefined' && process.versions?.node,
        isWebWorker: typeof importScripts === 'function',
        isServiceWorker: typeof self !== 'undefined' && 'serviceWorker' in self
    };
    
    console.log('Environment:', environment);
    return environment;
}

// Polyfill example - adding Web API to non-browser environment
// This is how libraries make Web APIs work in Node.js
if (typeof fetch === 'undefined') {
    // In Node.js, you might install 'node-fetch' package
    // global.fetch = require('node-fetch'); // Polyfill
}

// Standards bodies responsible for different parts:
const standards = {
    JavaScript: {
        maintainer: 'Ecma International (TC39)',
        specification: 'ECMAScript',
        includes: ['Variables', 'Functions', 'Objects', 'Promises', 'Classes'],
        environment: 'Any JavaScript runtime'
    },
    
    WebAPIs: {
        maintainer: 'W3C / WHATWG',
        specification: 'Various Web Standards',
        includes: ['DOM', 'Fetch', 'Canvas', 'Geolocation', 'Storage'],
        environment: 'Browsers only (unless polyfilled)'
    }
};

console.log('Standards breakdown:', standards);

// Real-world example: Isomorphic code (works in browser AND Node.js)
function isomorphicFunction(data) {
    // Only use core JavaScript features
    return data
        .filter(item => item.active)
        .map(item => ({
            id: item.id,
            name: item.name.toUpperCase(),
            created: new Date().toISOString()
        }))
        .sort((a, b) => a.name.localeCompare(b.name));
}

// This function works in:
// ✅ Browser
// ✅ Node.js  
// ✅ React Native
// ✅ Web Workers
// ✅ Any JavaScript environment

// Platform-specific adaptations:
function makeHttpRequest(url) {
    if (typeof fetch !== 'undefined') {
        // Browser environment - use Fetch API
        return fetch(url).then(r => r.json());
    } else if (typeof require !== 'undefined') {
        // Node.js environment - use built-in modules
        const https = require('https');
        return new Promise((resolve, reject) => {
            https.get(url, res => {
                let data = '';
                res.on('data', chunk => data += chunk);
                res.on('end', () => resolve(JSON.parse(data)));
                res.on('error', reject);
            });
        });
    } else {
        throw new Error('No HTTP client available in this environment');
    }
}

// Key takeaways:
// 1. JavaScript = Core language (ECMAScript)
// 2. Web APIs = Browser extensions to JavaScript
// 3. Web APIs don't work outside browsers (unless polyfilled)
// 4. Node.js has its own APIs (not Web APIs)
// 5. Write isomorphic code using only core JavaScript for portability
// 6. Use feature detection for Web APIs
// 7. Different standards bodies maintain different parts
```
</details>

**Intermediate: Q1** - How do Web APIs interact with the JavaScript event loop?

<details>
<summary>Answer</summary>

Web APIs work with the event loop by offloading operations to browser threads and queuing callbacks:

```javascript
// Understanding Web API + Event Loop interaction

console.log('1. Synchronous code starts');

// Web API: setTimeout
setTimeout(() => {
    console.log('4. setTimeout callback (macrotask)');
}, 0);

// Web API: Fetch  
fetch('https://jsonplaceholder.typicode.com/posts/1')
    .then(response => response.json())
    .then(data => {
        console.log('6. Fetch resolved (microtask)');
    });

// Web API: DOM Event
const button = document.createElement('button');
button.addEventListener('click', () => {
    console.log('Event callback (when clicked)');
});

// Promise (native JS, not Web API)
Promise.resolve().then(() => {
    console.log('5. Promise microtask');
});

console.log('2. Synchronous code continues');

// More synchronous code
for (let i = 0; i < 1000000; i++) {
    // Blocking the main thread
}

console.log('3. Synchronous code ends');

// Execution order:
// 1. Synchronous code starts
// 2. Synchronous code continues  
// 3. Synchronous code ends
// 4. setTimeout callback (macrotask)
// 5. Promise microtask
// 6. Fetch resolved (microtask)

// Detailed Web API flow demonstration:
function demonstrateWebAPIFlow() {
    console.log('\n=== Web API Flow Demo ===');
    
    // 1. Call Stack executes this function
    console.log('Step 1: Function executing on call stack');
    
    // 2. Web API call - browser handles this
    const timeoutId = setTimeout(() => {
        console.log('Step 4: Timeout callback from task queue');
        
        // Nested Web API call
        Promise.resolve().then(() => {
            console.log('Step 5: Nested microtask');
        });
        
    }, 0);
    
    // 3. Another Web API call
    fetch('/api/data')
        .then(() => {
            console.log('Step 6: Fetch microtask');
        })
        .catch(() => {
            console.log('Step 6: Fetch error (still microtask)');
        });
    
    console.log('Step 2: Function continues synchronously');
    
    // 4. Microtask (not Web API)
    queueMicrotask(() => {
        console.log('Step 3: Microtask queue');
    });
    
    console.log('Step 2.5: Function ending');
}

demonstrateWebAPIFlow();

// Browser threading model:
class WebAPIThreadingDemo {
    constructor() {
        this.setupEventListeners();
        this.demonstrateThreading();
    }
    
    setupEventListeners() {
        // Event listener registration (Web API)
        document.addEventListener('click', (event) => {
            console.log('Click event handled on main thread');
            console.log('Event target:', event.target.tagName);
        });
        
        // Multiple event types
        window.addEventListener('resize', () => {
            console.log('Resize event - UI thread responsive');
        });
    }
    
    demonstrateThreading() {
        console.log('\n=== Threading Demo ===');
        
        // Network request - handled by network thread
        this.makeNetworkRequest();
        
        // File operation - handled by I/O thread  
        this.simulateFileOperation();
        
        // Timer - handled by timer thread
        this.setupTimers();
        
        // Animation - handled by compositor thread
        this.startAnimation();
    }
    
    makeNetworkRequest() {
        const start = performance.now();
        
        // Network thread handles the actual request
        fetch('https://httpbin.org/delay/2')
            .then(response => {
                const end = performance.now();
                console.log(`Network request completed in ${end - start}ms`);
                return response.json();
            })
            .then(data => {
                // This callback runs on main thread
                console.log('Processing response on main thread');
            })
            .catch(error => {
                console.log('Network error handled on main thread');
            });
        
        console.log('Network request initiated - non-blocking');
    }
    
    simulateFileOperation() {
        // Simulating file read (in real browser, would use File API)
        const fileInput = document.createElement('input');
        fileInput.type = 'file';
        
        fileInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                
                // File reading handled by I/O thread
                reader.onload = (e) => {
                    console.log('File read completed on main thread');
                    console.log('File size:', e.target.result.length);
                };
                
                reader.readAsText(file);
                console.log('File reading initiated - non-blocking');
            }
        });
    }
    
    setupTimers() {
        // Different timer types
        setTimeout(() => {
            console.log('Single timeout executed');
        }, 1000);
        
        const intervalId = setInterval(() => {
            console.log('Interval tick');
        }, 2000);
        
        // Clean up after 10 seconds
        setTimeout(() => {
            clearInterval(intervalId);
            console.log('Interval cleared');
        }, 10000);
        
        // requestAnimationFrame - special timer for animations
        let animationFrame = 0;
        function animate() {
            animationFrame++;
            console.log(`Animation frame: ${animationFrame}`);
            
            if (animationFrame < 60) { // Run for ~1 second at 60fps
                requestAnimationFrame(animate);
            }
        }
        
        requestAnimationFrame(animate);
    }
    
    startAnimation() {
        const element = document.createElement('div');
        element.style.cssText = `
            position: fixed; 
            top: 10px; 
            left: 10px; 
            width: 50px; 
            height: 50px; 
            background: red;
            transition: transform 2s;
        `;
        document.body.appendChild(element);
        
        // CSS animations run on compositor thread (when possible)
        setTimeout(() => {
            element.style.transform = 'translateX(200px)';
        }, 100);
    }
}

// Task queue priorities:
function demonstrateTaskPriorities() {
    console.log('\n=== Task Priority Demo ===');
    
    // Macrotasks (Task Queue)
    setTimeout(() => console.log('Timeout 1'), 0);
    setTimeout(() => console.log('Timeout 2'), 0);
    
    // Microtasks (Job Queue) - higher priority
    Promise.resolve().then(() => console.log('Promise 1'));
    Promise.resolve().then(() => console.log('Promise 2'));
    
    // queueMicrotask - also microtask
    queueMicrotask(() => console.log('queueMicrotask'));
    
    // Immediate microtask
    Promise.resolve().then(() => {
        console.log('Promise 3');
        
        // Nested microtask
        Promise.resolve().then(() => console.log('Nested Promise'));
        
        // Nested macrotask
        setTimeout(() => console.log('Nested Timeout'), 0);
    });
    
    console.log('Synchronous end');
    
    // Expected output:
    // Synchronous end
    // Promise 1
    // Promise 2  
    // queueMicrotask
    // Promise 3
    // Nested Promise
    // Timeout 1
    // Timeout 2
    // Nested Timeout
}

demonstrateTaskPriorities();

// Performance implications:
function performanceImplications() {
    console.log('\n=== Performance Implications ===');
    
    // Heavy computation blocks event loop
    function heavyComputation() {
        const start = performance.now();
        let result = 0;
        
        // This blocks the main thread
        for (let i = 0; i < 10000000; i++) {
            result += Math.random();
        }
        
        const end = performance.now();
        console.log(`Heavy computation: ${end - start}ms`);
        return result;
    }
    
    // Non-blocking approach using Web Workers
    function nonBlockingComputation() {
        if (typeof Worker !== 'undefined') {
            const worker = new Worker(URL.createObjectURL(new Blob([`
                self.onmessage = function(e) {
                    let result = 0;
                    for (let i = 0; i < 10000000; i++) {
                        result += Math.random();
                    }
                    self.postMessage(result);
                };
            `], { type: 'application/javascript' })));
            
            worker.postMessage('start');
            worker.onmessage = (e) => {
                console.log('Worker result:', e.data);
                worker.terminate();
            };
            
            console.log('Computation offloaded to worker thread');
        }
    }
    
    // Demonstrate the difference
    setTimeout(() => {
        console.log('This timer should fire promptly');
    }, 100);
    
    // This will delay the timer
    // heavyComputation();
    
    // This won't delay the timer
    nonBlockingComputation();
}

performanceImplications();

// Key concepts:
// 1. Web APIs run in separate browser threads
// 2. Callbacks are queued when Web API operations complete  
// 3. Event loop processes queues when call stack is empty
// 4. Microtasks have higher priority than macrotasks
// 5. Blocking code delays all queued callbacks
// 6. Use Web Workers for heavy computations
// 7. requestAnimationFrame syncs with display refresh
```
</details>

**Intermediate: Q2** - What is the difference between browser APIs and Node.js APIs?

<details>
<summary>Answer</summary>

Browser APIs and Node.js APIs serve different environments with different capabilities and constraints:

```javascript
// BROWSER APIs (Web APIs) - Client-side environment
// Available in: Chrome, Firefox, Safari, Edge, etc.

if (typeof window !== 'undefined') {
    console.log('=== BROWSER ENVIRONMENT ===');
    
    // DOM APIs - Manipulate web pages
    const element = document.createElement('div');
    element.innerHTML = '<h1>Browser API Demo</h1>';
    document.body.appendChild(element);
    
    // Browser Storage APIs
    localStorage.setItem('userPreference', 'dark-theme');
    sessionStorage.setItem('tempData', 'session-value');
    
    // Network APIs (browser-specific behavior)
    fetch('/api/data', {
        credentials: 'include', // Cookies included
        mode: 'cors' // CORS handling
    })
    .then(response => response.json())
    .then(data => console.log('Browser fetch:', data));
    
    // Geolocation API
    navigator.geolocation.getCurrentPosition(position => {
        console.log('User location:', position.coords);
    });
    
    // Camera/Microphone APIs
    navigator.mediaDevices.getUserMedia({ video: true })
        .then(stream => {
            const video = document.querySelector('video');
            video.srcObject = stream;
        });
    
    // Notification API
    Notification.requestPermission().then(permission => {
        if (permission === 'granted') {
            new Notification('Hello from browser!');
        }
    });
    
    // Service Worker (browser background processing)
    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('/sw.js');
    }
    
    // Browser-specific constraints:
    // - Sandboxed environment (security restrictions)
    // - No direct file system access
    // - Same-origin policy
    // - CORS restrictions
    // - Limited hardware access
}

// ============================================

// NODE.JS APIs - Server-side environment  
// Available in: Node.js runtime

if (typeof process !== 'undefined' && process.versions?.node) {
    console.log('=== NODE.JS ENVIRONMENT ===');
    
    // File System API - Full file access
    const fs = require('fs');
    const path = require('path');
    
    // Read/write files directly
    fs.writeFile('example.txt', 'Hello Node.js!', (err) => {
        if (!err) {
            fs.readFile('example.txt', 'utf8', (err, data) => {
                console.log('File content:', data);
            });
        }
    });
    
    // Operating System APIs
    const os = require('os');
    console.log('OS Info:', {
        platform: os.platform(),
        arch: os.arch(),
        totalMemory: os.totalmem(),
        freeMemory: os.freemem(),
        cpuCount: os.cpus().length
    });
    
    // Process APIs
    console.log('Process Info:', {
        pid: process.pid,
        nodeVersion: process.version,
        platform: process.platform,
        cwd: process.cwd(),
        memoryUsage: process.memoryUsage()
    });
    
    // Network APIs (more powerful than browser)
    const http = require('http');
    const server = http.createServer((req, res) => {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Hello from Node.js server!' }));
    });
    
    // Child Process APIs - Run external programs
    const { exec, spawn } = require('child_process');
    
    exec('ls -la', (error, stdout, stderr) => {
        if (!error) {
            console.log('Directory listing:', stdout);
        }
    });
    
    // Crypto APIs (built-in, not Web Crypto)
    const crypto = require('crypto');
    const hash = crypto.createHash('sha256');
    hash.update('Hello Node.js');
    console.log('Hash:', hash.digest('hex'));
    
    // Stream APIs - Handle large data
    const stream = require('stream');
    const { Transform } = stream;
    
    const upperCaseTransform = new Transform({
        transform(chunk, encoding, callback) {
            this.push(chunk.toString().toUpperCase());
            callback();
        }
    });
    
    // Node.js capabilities:
    // - Full file system access
    // - Native module loading
    // - Process control
    // - System-level operations
    // - Direct hardware access
    // - No security sandbox
}

// ============================================

// COMPARISON TABLE
const apiComparison = {
    'File Access': {
        browser: 'Limited (File API, drag-drop only)',
        nodejs: 'Full file system access (fs module)'
    },
    
    'Network': {
        browser: 'Fetch API (CORS restricted)',
        nodejs: 'http/https modules (no restrictions)'
    },
    
    'Storage': {
        browser: 'localStorage, sessionStorage, IndexedDB',
        nodejs: 'Direct file/database access'
    },
    
    'Hardware': {
        browser: 'Limited (camera, GPS via permissions)',
        nodejs: 'Full system access'
    },
    
    'Security': {
        browser: 'Sandboxed, same-origin policy',
        nodejs: 'Full system privileges'
    },
    
    'UI': {
        browser: 'DOM, Canvas, WebGL',
        nodejs: 'CLI, terminal output only'
    },
    
    'Modules': {
        browser: 'ES modules, script tags',
        nodejs: 'CommonJS, ES modules, npm packages'
    }
};

console.log('API Comparison:', apiComparison);

// ============================================

// ISOMORPHIC CODE - Works in both environments
function createIsomorphicHTTPClient() {
    // Detect environment and use appropriate API
    
    if (typeof fetch !== 'undefined') {
        // Browser environment
        return {
            get: url => fetch(url).then(r => r.json()),
            post: (url, data) => fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(data)
            }).then(r => r.json())
        };
    } 
    else if (typeof require !== 'undefined') {
        // Node.js environment
        const https = require('https');
        const http = require('http');
        const { URL } = require('url');
        
        return {
            get: url => {
                return new Promise((resolve, reject) => {
                    const client = url.startsWith('https') ? https : http;
                    client.get(url, res => {
                        let data = '';
                        res.on('data', chunk => data += chunk);
                        res.on('end', () => resolve(JSON.parse(data)));
                        res.on('error', reject);
                    });
                });
            },
            
            post: (url, postData) => {
                return new Promise((resolve, reject) => {
                    const parsedUrl = new URL(url);
                    const client = parsedUrl.protocol === 'https:' ? https : http;
                    
                    const options = {
                        hostname: parsedUrl.hostname,
                        port: parsedUrl.port,
                        path: parsedUrl.pathname,
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                            'Content-Length': Buffer.byteLength(JSON.stringify(postData))
                        }
                    };
                    
                    const req = client.request(options, res => {
                        let data = '';
                        res.on('data', chunk => data += chunk);
                        res.on('end', () => resolve(JSON.parse(data)));
                    });
                    
                    req.on('error', reject);
                    req.write(JSON.stringify(postData));
                    req.end();
                });
            }
        };
    }
}

// ============================================

// POLYFILLS - Making APIs work across environments

// Node.js polyfill for browser APIs
if (typeof global !== 'undefined' && !global.localStorage) {
    // Simple localStorage polyfill for Node.js
    const fs = require('fs');
    const storageFile = './localstorage.json';
    
    global.localStorage = {
        getItem: (key) => {
            try {
                const data = JSON.parse(fs.readFileSync(storageFile, 'utf8'));
                return data[key] || null;
            } catch {
                return null;
            }
        },
        
        setItem: (key, value) => {
            try {
                let data = {};
                if (fs.existsSync(storageFile)) {
                    data = JSON.parse(fs.readFileSync(storageFile, 'utf8'));
                }
                data[key] = value;
                fs.writeFileSync(storageFile, JSON.stringify(data));
            } catch (error) {
                console.error('localStorage polyfill error:', error);
            }
        }
    };
}

// Browser polyfill for Node.js APIs (limited)
if (typeof window !== 'undefined' && !window.process) {
    // Minimal process polyfill
    window.process = {
        env: {},
        platform: navigator.platform,
        version: 'browser',
        nextTick: (callback) => setTimeout(callback, 0)
    };
}

// ============================================

// ENVIRONMENT DETECTION UTILITY
function detectEnvironment() {
    const env = {
        isBrowser: typeof window !== 'undefined',
        isNode: typeof process !== 'undefined' && process.versions?.node,
        isWebWorker: typeof importScripts === 'function',
        isServiceWorker: typeof self !== 'undefined' && 'serviceWorker' in self,
        
        // Available APIs
        apis: {
            dom: typeof document !== 'undefined',
            fetch: typeof fetch !== 'undefined',
            localStorage: typeof localStorage !== 'undefined',
            geolocation: typeof navigator !== 'undefined' && 'geolocation' in navigator,
            fileSystem: typeof require !== 'undefined',
            crypto: typeof crypto !== 'undefined',
            webWorkers: typeof Worker !== 'undefined'
        }
    };
    
    return env;
}

console.log('Environment Detection:', detectEnvironment());

// Key Differences Summary:
// 1. Browser: UI-focused, sandboxed, user-permission based
// 2. Node.js: System-focused, full access, server-oriented
// 3. Browser: Real-time user interaction, visual output
// 4. Node.js: Batch processing, file operations, networking
// 5. Browser: Web standards (W3C, WHATWG)
// 6. Node.js: System APIs, POSIX-like operations
// 7. Security: Browser restricts, Node.js trusts
// 8. Performance: Browser optimizes for responsiveness, Node.js for throughput
```
</details>

## Execution context

**Beginner: Q1** - What is an execution context in JavaScript?

<details>
<summary>Answer</summary>

An execution context is the environment where JavaScript code is executed, containing all information needed to run that code:

```javascript
// Execution context contains:
// 1. Variable Environment (var declarations, function declarations)
// 2. Lexical Environment (let/const, function parameters)
// 3. this binding
// 4. Outer environment reference

function example() {
    var a = 1;        // Variable Environment
    let b = 2;        // Lexical Environment  
    const c = 3;      // Lexical Environment
    
    console.log(this); // this binding
    console.log(a, b, c);
}

// Global execution context
var globalVar = 'global';

// Function execution context created when called
example(); // Creates new execution context
```
</details>

**Beginner: Q2** - What are the different types of execution contexts?

<details>
<summary>Answer</summary>

Three types: **Global**, **Function**, and **Eval** execution contexts:

```javascript
// 1. GLOBAL EXECUTION CONTEXT
// Created when script starts, only one per program
var globalVariable = 'I am global';
console.log('Global context');

// 2. FUNCTION EXECUTION CONTEXT  
// Created each time a function is called
function functionContext() {
    var localVariable = 'I am local';
    console.log('Function context');
}

functionContext(); // Creates function execution context

// 3. EVAL EXECUTION CONTEXT
// Created when eval() is executed (avoid using eval)
eval('var evalVar = "I am eval"; console.log("Eval context");');

// Each function call creates its own context:
function outer() {
    console.log('Outer context');
    
    function inner() {
        console.log('Inner context');
    }
    
    inner(); // Creates separate inner context
}

outer(); // Creates outer context, then inner context

// Recursive functions create multiple contexts:
function countdown(n) {
    console.log(`Context for n=${n}`);
    if (n > 0) {
        countdown(n - 1); // Creates new context for each call
    }
}

countdown(3); // Creates 4 contexts (3, 2, 1, 0)
```
</details>

**Beginner: Q3** - When is a new execution context created?

<details>
<summary>Answer</summary>

New execution contexts are created when:

```javascript
// 1. SCRIPT STARTS - Global context created
console.log('Global execution context created');

// 2. FUNCTION CALL - Function context created
function myFunction() {
    console.log('Function execution context created');
}

myFunction(); // New context created here

// 3. METHOD CALL - Function context created  
const obj = {
    method() {
        console.log('Method execution context created');
    }
};

obj.method(); // New context created

// 4. CONSTRUCTOR CALL - Function context created
function Constructor() {
    this.value = 'Constructor execution context created';
}

new Constructor(); // New context created

// 5. RECURSIVE CALLS - New context per call
function recursive(n) {
    if (n > 0) {
        recursive(n - 1); // Each call creates new context
    }
}

recursive(3); // Creates 4 contexts

// 6. CALLBACK FUNCTIONS - New context when called
setTimeout(function() {
    console.log('Callback execution context created');
}, 0);

// 7. EVENT HANDLERS - New context per event
document.addEventListener('click', function() {
    console.log('Event handler execution context created');
});

// NOT created for:
// - Variable assignments
// - Property access
// - Arithmetic operations
var x = 10;     // No new context
obj.prop;       // No new context
x + 5;          // No new context
```
</details>

**Intermediate: Q1** - What are the phases of execution context creation?

<details>
<summary>Answer</summary>

Two phases: **Creation Phase** and **Execution Phase**:

```javascript
// CREATION PHASE:
// 1. Create Variable Environment
// 2. Create Lexical Environment  
// 3. Determine 'this' binding

function demonstratePhases() {
    // CREATION PHASE happens first:
    // - hoistFunction is created and assigned
    // - var variables are created but undefined
    // - let/const variables are created but uninitialized (TDZ)
    
    console.log(typeof hoistFunction); // 'function' - already created
    console.log(varVariable);          // undefined - created but not assigned
    // console.log(letVariable);       // ReferenceError - in TDZ
    
    // EXECUTION PHASE happens second:
    var varVariable = 'now assigned';
    let letVariable = 'now initialized';
    const constVariable = 'now initialized';
    
    function hoistFunction() {
        return 'I was hoisted';
    }
}

demonstratePhases();

// Detailed breakdown:
function detailedExample() {
    // === CREATION PHASE ===
    // 1. Variable Environment created:
    //    - hoistedFunc: function object
    //    - varA: undefined
    //    - arguments: arguments object
    
    // 2. Lexical Environment created:
    //    - letB: <uninitialized> (TDZ)
    //    - constC: <uninitialized> (TDZ)
    
    // 3. this binding determined
    
    console.log('=== Creation Phase Complete ===');
    
    // === EXECUTION PHASE ===
    console.log(hoistedFunc()); // Works - function already created
    console.log(varA);          // undefined - created but not assigned
    
    var varA = 'assigned';      // Now assigned
    let letB = 'initialized';   // Now initialized, out of TDZ
    const constC = 'initialized'; // Now initialized, out of TDZ
    
    function hoistedFunc() {
        return 'Function declaration hoisted';
    }
    
    console.log('=== Execution Phase Complete ===');
}

detailedExample();

// Global context phases:
// CREATION PHASE:
console.log(typeof globalFunc); // 'function'
console.log(globalVar);          // undefined

// EXECUTION PHASE:  
var globalVar = 'global value';

function globalFunc() {
    return 'global function';
}
```
</details>

**Intermediate: Q2** - How does the execution context stack work?

<details>
<summary>Answer</summary>

The execution context stack (call stack) manages execution contexts using LIFO (Last In, First Out):

```javascript
// Call Stack Demonstration

function first() {
    console.log('1. First function starts');
    second(); // Push second() to stack
    console.log('5. First function resumes');
} // Pop first() from stack

function second() {
    console.log('2. Second function starts');
    third(); // Push third() to stack  
    console.log('4. Second function resumes');
} // Pop second() from stack

function third() {
    console.log('3. Third function executes');
} // Pop third() from stack

// Call stack visualization:
console.log('0. Global context (bottom of stack)');
first(); // Push first() to stack
console.log('6. Back to global context');

// Stack states during execution:
// Start:    [Global]
// Call 1:   [Global, first]
// Call 2:   [Global, first, second]  
// Call 3:   [Global, first, second, third]
// Return 3: [Global, first, second]
// Return 2: [Global, first]
// Return 1: [Global]

// Recursive stack example:
function factorial(n) {
    console.log(`Stack depth: ${n}`);
    
    if (n <= 1) {
        console.log('Base case reached, unwinding stack');
        return 1;
    }
    
    return n * factorial(n - 1); // Each call adds to stack
}

console.log('Result:', factorial(4));

// Stack overflow example (don't run):
// function infiniteRecursion() {
//     infiniteRecursion(); // Stack grows until memory limit
// }
// infiniteRecursion(); // RangeError: Maximum call stack size exceeded

// Async operations and stack:
function asyncExample() {
    console.log('1. Sync operation');
    
    setTimeout(() => {
        console.log('3. Async callback (new stack)');
    }, 0);
    
    console.log('2. Sync operation continues');
}

asyncExample();

// Stack trace example:
function a() { b(); }
function b() { c(); }  
function c() { throw new Error('Stack trace demo'); }

try {
    a();
} catch (error) {
    console.log('Stack trace:');
    console.log(error.stack);
    // Shows: c() -> b() -> a() -> global
}

// Practical stack management:
function processLargeArray(arr) {
    // Avoid deep recursion for large arrays
    if (arr.length > 1000) {
        // Use iterative approach instead
        return arr.reduce((sum, item) => sum + item, 0);
    } else {
        // Safe to use recursion
        return arr.length === 0 ? 0 : arr[0] + processLargeArray(arr.slice(1));
    }
}
```
</details>

## Lexical environment

**Beginner: Q1** - What is a lexical environment?

<details>
<summary>Answer</summary>

A lexical environment is a structure that holds identifier-variable mappings and a reference to the outer environment:

```javascript
// Lexical Environment = Environment Record + Outer Reference

function outerFunction() {
    var outerVar = 'outer';
    
    // Outer function's lexical environment:
    // Environment Record: { outerVar: 'outer' }
    // Outer Reference: Global Environment
    
    function innerFunction() {
        var innerVar = 'inner';
        
        // Inner function's lexical environment:
        // Environment Record: { innerVar: 'inner' }
        // Outer Reference: Outer Function Environment
        
        console.log(innerVar); // Found in current environment
        console.log(outerVar); // Found in outer environment
    }
    
    return innerFunction;
}

// Global lexical environment:
// Environment Record: { outerFunction: function, globalVar: 'global' }
// Outer Reference: null

var globalVar = 'global';
const closure = outerFunction();
closure(); // Accesses preserved lexical environments

// Let/const create their own lexical environment in blocks:
function blockExample() {
    var functionScoped = 'function';
    
    {
        let blockScoped = 'block';
        const alsoBlockScoped = 'also block';
        
        // Block lexical environment:
        // Environment Record: { blockScoped: 'block', alsoBlockScoped: 'also block' }
        // Outer Reference: Function Environment
        
        console.log(functionScoped); // Found in outer environment
        console.log(blockScoped);    // Found in current environment
    }
    
    // blockScoped no longer accessible - environment destroyed
}

blockExample();
```
</details>

**Beginner: Q2** - How is lexical environment related to scope?

<details>
<summary>Answer</summary>

Lexical environment implements scope - it determines what variables are accessible at any point in code:

```javascript
// Lexical environment = The mechanism that implements scope

const globalScope = 'global';

function demonstrateScope() {
    const functionScope = 'function';
    
    // Function's lexical environment contains functionScope
    // and references global environment for globalScope
    
    if (true) {
        const blockScope = 'block';
        let anotherBlockVar = 'another';
        
        // Block's lexical environment contains block variables
        // and references function environment
        
        console.log(blockScope);     // Current environment
        console.log(functionScope);  // Outer environment (function)
        console.log(globalScope);    // Outer environment (global)
    }
    
    // blockScope not accessible here - different lexical environment
    console.log(functionScope);  // Still accessible
    console.log(globalScope);    // Still accessible
}

demonstrateScope();

// Scope chain = Chain of lexical environments
function outer() {
    const outerVar = 'outer';
    
    function middle() {
        const middleVar = 'middle';
        
        function inner() {
            const innerVar = 'inner';
            
            // Scope chain: inner → middle → outer → global
            // Each step is a lexical environment reference
            
            console.log(innerVar);   // Current lexical environment
            console.log(middleVar);  // Parent lexical environment
            console.log(outerVar);   // Grandparent lexical environment
        }
        
        return inner;
    }
    
    return middle();
}

const nested = outer();
nested(); // Scope chain preserved through closures

// Variable resolution through lexical environments:
let x = 'global x';

function scopeResolution() {
    let x = 'function x';
    
    {
        let x = 'block x';
        console.log(x); // 'block x' - found in current lexical environment
    }
    
    console.log(x); // 'function x' - block environment destroyed
}

scopeResolution();
console.log(x); // 'global x' - function environment destroyed
```
</details>

**Beginner: Q3** - What happens to variables in a lexical environment?

<details>
<summary>Answer</summary>

Variables are stored in the environment record and follow specific lifecycle rules:

```javascript
// Variable lifecycle in lexical environments:

function variableLifecycle() {
    // CREATION PHASE:
    // var variables: created and initialized with undefined
    // let/const variables: created but uninitialized (TDZ)
    // function declarations: created and initialized
    
    console.log(typeof varVariable);    // 'undefined' - created, not assigned
    console.log(typeof funcDeclaration); // 'function' - created and assigned
    // console.log(letVariable);        // ReferenceError - in TDZ
    
    // EXECUTION PHASE:
    var varVariable = 'assigned';
    let letVariable = 'initialized';
    const constVariable = 'initialized';
    
    function funcDeclaration() {
        return 'hoisted';
    }
    
    // Variables accessible in this environment
    console.log(varVariable, letVariable, constVariable);
}

variableLifecycle();

// Block-scoped variables create new lexical environments:
function blockScoping() {
    var outerVar = 'outer';
    
    {
        // New lexical environment created for block
        let blockVar = 'block';
        const anotherBlockVar = 'another';
        
        console.log(blockVar);      // Accessible in block
        console.log(outerVar);      // Inherited from outer environment
    }
    
    // Block lexical environment destroyed
    // console.log(blockVar);       // ReferenceError - environment gone
    console.log(outerVar);          // Still accessible
}

blockScoping();

// Variable updates in lexical environments:
function variableUpdates() {
    let count = 0;
    
    function increment() {
        count++; // Updates variable in outer lexical environment
        return count;
    }
    
    function getCount() {
        return count; // Reads from outer lexical environment
    }
    
    return { increment, getCount };
}

const counter = variableUpdates();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2

// Garbage collection of lexical environments:
function environmentCleanup() {
    let largeData = new Array(1000000);
    
    function useData() {
        return largeData.length; // Keeps largeData alive
    }
    
    function noData() {
        return 'no reference to largeData';
    }
    
    // If only noData is returned, largeData can be garbage collected
    return noData; // largeData environment can be cleaned up
}

const cleanup = environmentCleanup();

// Loop variables and lexical environments:
for (let i = 0; i < 3; i++) {
    // Each iteration creates new lexical environment for 'i'
    setTimeout(() => console.log('let:', i), 100); // 0, 1, 2
}

for (var j = 0; j < 3; j++) {
    // var shares same lexical environment (function-scoped)
    setTimeout(() => console.log('var:', j), 200); // 3, 3, 3
}
```
</details>

**Intermediate: Q1** - Explain the relationship between lexical environment and closures.

<details>
<summary>Answer</summary>

Closures are created when functions retain access to their lexical environment even after the outer function returns:

```javascript
// Closure = Function + Preserved Lexical Environment

function createClosure() {
    let privateVar = 'private';
    let count = 0;
    
    // Inner function captures outer lexical environment
    function closureFunction() {
        count++;
        return `${privateVar} - called ${count} times`;
    }
    
    // When returned, closureFunction keeps reference to lexical environment
    return closureFunction;
}

const closure = createClosure();
// createClosure execution context is destroyed, but lexical environment preserved

console.log(closure()); // "private - called 1 times"
console.log(closure()); // "private - called 2 times"

// Multiple closures share same lexical environment:
function sharedEnvironment() {
    let shared = 0;
    
    return {
        increment: () => ++shared,
        decrement: () => --shared,
        getValue: () => shared
    };
}

const counter = sharedEnvironment();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1
console.log(counter.getValue());  // 1

// Closure with parameters captures both:
function multiplierFactory(factor) {
    // Lexical environment: { factor: parameter value }
    
    return function(number) {
        // Closure captures 'factor' from outer environment
        return number * factor;
    };
}

const double = multiplierFactory(2);
const triple = multiplierFactory(3);

console.log(double(5)); // 10 - uses factor=2
console.log(triple(5)); // 15 - uses factor=3

// Nested closures create chain of environments:
function outermost(a) {
    return function middle(b) {
        return function innermost(c) {
            // Closure captures all three lexical environments
            return a + b + c;
        };
    };
}

const nested = outermost(1)(2);
console.log(nested(3)); // 6 - accesses a=1, b=2, c=3

// Loop closures and lexical environment:
function createFunctions() {
    const functions = [];
    
    // Each iteration creates new lexical environment with let
    for (let i = 0; i < 3; i++) {
        functions.push(() => i); // Captures current 'i' value
    }
    
    return functions;
}

const funcs = createFunctions();
console.log(funcs[0]()); // 0
console.log(funcs[1]()); // 1
console.log(funcs[2]()); // 2

// Module pattern with closures:
const module = (function() {
    // Private lexical environment
    let privateState = 'secret';
    let counter = 0;
    
    // Public interface (closures)
    return {
        getState: () => privateState,
        increment: () => ++counter,
        getCount: () => counter
    };
})();

console.log(module.getState()); // 'secret'
console.log(module.increment()); // 1
// console.log(privateState); // ReferenceError - not accessible
```
</details>

**Intermediate: Q2** - How do `let` and `const` create temporal dead zones in lexical environments?

<details>
<summary>Answer</summary>

TDZ occurs because `let`/`const` variables exist in the lexical environment but are uninitialized until declaration:

```javascript
// Temporal Dead Zone = Time between environment creation and initialization

function demonstrateTDZ() {
    // TDZ starts here for letVar and constVar
    
    console.log(typeof varVar);     // 'undefined' - hoisted and initialized
    // console.log(letVar);         // ReferenceError - in TDZ
    // console.log(constVar);       // ReferenceError - in TDZ
    
    var varVar = 'var assigned';    // var works normally
    
    // TDZ continues...
    
    let letVar = 'let assigned';    // TDZ ends for letVar
    const constVar = 'const assigned'; // TDZ ends for constVar
    
    console.log(letVar, constVar);  // Now accessible
}

demonstrateTDZ();

// Block-level TDZ:
function blockTDZ() {
    let outer = 'outer';
    
    {
        // TDZ starts for blockVar
        console.log(outer);         // Accessible from outer scope
        // console.log(blockVar);   // ReferenceError - TDZ
        
        let blockVar = 'block';     // TDZ ends
        console.log(blockVar);      // Now accessible
    }
    
    // blockVar no longer accessible - lexical environment destroyed
}

blockTDZ();

// Function parameters and TDZ:
function parameterTDZ(a = b, b = 1) {
    // Parameter 'a' tries to access 'b' before it's initialized
    return a + b;
}

// parameterTDZ(); // ReferenceError - b is in TDZ when a is initialized

// Correct version:
function correctParameters(a = 1, b = a) {
    return a + b; // Works - a is initialized before b uses it
}

console.log(correctParameters()); // 2

// Class TDZ:
// console.log(MyClass); // ReferenceError - class in TDZ

class MyClass {
    constructor() {
        this.value = 'initialized';
    }
}

console.log(MyClass); // Now accessible

// typeof operator and TDZ:
function typeofTDZ() {
    // typeof usually returns 'undefined' for undeclared variables
    console.log(typeof undeclaredVar); // 'undefined' - safe
    
    // But with let/const in TDZ, it throws error
    // console.log(typeof letInTDZ); // ReferenceError - not 'undefined'!
    
    let letInTDZ = 'value';
    console.log(typeof letInTDZ); // 'string' - after initialization
}

typeofTDZ();

// Switch statement TDZ (common gotcha):
function switchTDZ(option) {
    switch (option) {
        case 1:
            let shared = 'case 1';
            console.log(shared);
            break;
            
        case 2:
            // shared is in TDZ here - same lexical environment!
            // console.log(shared); // ReferenceError
            shared = 'case 2'; // Can't assign before declaration
            break;
    }
}

// switchTDZ(2); // ReferenceError

// Solution - use block scopes:
function switchFixed(option) {
    switch (option) {
        case 1: {
            let shared = 'case 1';
            console.log(shared);
            break;
        }
        case 2: {
            let shared = 'case 2'; // Different lexical environment
            console.log(shared);
            break;
        }
    }
}

switchFixed(2); // Works fine

// TDZ and function hoisting:
function hoistingWithTDZ() {
    // Function declarations are fully hoisted
    console.log(hoistedFunc()); // Works
    
    // Function expressions with let/const are in TDZ
    // console.log(funcExpr()); // ReferenceError - TDZ
    
    function hoistedFunc() {
        return 'hoisted';
    }
    
    const funcExpr = function() {
        return 'expression';
    };
    
    console.log(funcExpr()); // Now works
}

hoistingWithTDZ();
```
</details>

## Scope chain

**Beginner: Q1** - What is the scope chain?

<details>
<summary>Answer</summary>

The scope chain is a mechanism that determines how JavaScript resolves variable names by traversing linked lexical environments:

```javascript
// Scope chain = Chain of lexical environments for variable resolution

const globalVar = 'global';

function outerFunction() {
    const outerVar = 'outer';
    
    function innerFunction() {
        const innerVar = 'inner';
        
        // Scope chain: innerFunction → outerFunction → global
        console.log(innerVar);   // Found in current scope
        console.log(outerVar);   // Found in parent scope
        console.log(globalVar);  // Found in global scope
    }
    
    innerFunction();
}

outerFunction();

// Visual representation of scope chain lookup:
function scopeChainDemo() {
    const level1 = 'first level';
    
    function level2() {
        const level2Var = 'second level';
        
        function level3() {
            const level3Var = 'third level';
            
            // When accessing a variable, JavaScript searches:
            // 1. Current scope (level3)
            // 2. Parent scope (level2) 
            // 3. Grandparent scope (level1)
            // 4. Global scope
            // 5. If not found: ReferenceError
            
            console.log(level3Var);  // Current scope
            console.log(level2Var);  // Parent scope
            console.log(level1);     // Grandparent scope
            console.log(globalVar);  // Global scope
        }
        
        level3();
    }
    
    level2();
}

scopeChainDemo();
```
</details>

**Beginner: Q2** - How does JavaScript resolve variable names using the scope chain?

<details>
<summary>Answer</summary>

JavaScript resolves variables by searching from innermost to outermost scope until found or throwing ReferenceError:

```javascript
// Variable resolution algorithm:
// 1. Check current lexical environment
// 2. If not found, check outer environment
// 3. Repeat until found or reach global scope
// 4. If not found in global: ReferenceError

const globalVariable = 'global value';

function demonstrateResolution() {
    const outerVariable = 'outer value';
    
    function innerFunction() {
        const innerVariable = 'inner value';
        
        // Resolution examples:
        console.log(innerVariable);   // Found in current scope (immediate)
        console.log(outerVariable);   // Found in parent scope
        console.log(globalVariable);  // Found in global scope
        
        try {
            console.log(undefinedVariable); // Not found anywhere - ReferenceError
        } catch (e) {
            console.log('Variable not found:', e.message);
        }
    }
    
    innerFunction();
}

demonstrateResolution();

// Variable shadowing in resolution:
const shadowedVar = 'global version';

function shadowingDemo() {
    const shadowedVar = 'function version'; // Shadows global
    
    function inner() {
        const shadowedVar = 'inner version'; // Shadows function
        
        // Resolution stops at first match
        console.log(shadowedVar); // 'inner version'
    }
    
    inner();
    console.log(shadowedVar); // 'function version'
}

shadowingDemo();
console.log(shadowedVar); // 'global version'

// Resolution with different declaration types:
function resolutionTypes() {
    var varVariable = 'var in function';
    
    {
        let blockVariable = 'let in block';
        const constVariable = 'const in block';
        
        function blockFunction() {
            // Resolution finds block-scoped variables
            console.log(varVariable);    // Function scope
            console.log(blockVariable);  // Block scope
            console.log(constVariable);  // Block scope
        }
        
        blockFunction();
    }
    
    function afterBlock() {
        console.log(varVariable);        // Still accessible
        // console.log(blockVariable);   // ReferenceError - not in scope
    }
    
    afterBlock();
}

resolutionTypes();
```
</details>

**Beginner: Q3** - What happens when a variable is not found in the current scope?

<details>
<summary>Answer</summary>

When a variable is not found in current scope, JavaScript searches up the scope chain and throws ReferenceError if not found anywhere:

```javascript
// Variable not found behavior:
// 1. Search current scope
// 2. Search parent scopes (scope chain)
// 3. Search global scope
// 4. Throw ReferenceError if not found

function demonstrateNotFound() {
    const localVar = 'local';
    
    function inner() {
        console.log(localVar); // ✅ Found in parent scope
        
        try {
            console.log(unknownVariable); // ❌ Not found anywhere
        } catch (error) {
            console.log('Error type:', error.constructor.name); // ReferenceError
            console.log('Error message:', error.message);
        }
    }
    
    inner();
}

demonstrateNotFound();

// Different scenarios of "not found":

// 1. Completely undeclared variable
function undeclaredVariable() {
    try {
        console.log(totallyUndeclared);
    } catch (e) {
        console.log('Undeclared error:', e.message);
    }
}

undeclaredVariable();

// 2. Variable declared but not in scope
function outOfScope() {
    {
        let blockScoped = 'only in block';
    }
    
    try {
        console.log(blockScoped);
    } catch (e) {
        console.log('Out of scope error:', e.message);
    }
}

outOfScope();

// 3. Variable in temporal dead zone
function temporalDeadZone() {
    try {
        console.log(tdzVariable);
    } catch (e) {
        console.log('TDZ error:', e.message);
    }
    
    let tdzVariable = 'now initialized';
}

temporalDeadZone();

// typeof operator with non-existent variables:
function typeofBehavior() {
    console.log(typeof undeclaredVar); // "undefined" (no error!)
    
    // But let/const in TDZ still throw ReferenceError
    try {
        console.log(typeof letInTDZ);
    } catch (e) {
        console.log('typeof TDZ error:', e.message);
    }
    
    let letInTDZ = 'value';
}

typeofBehavior();

// Best practices for handling missing variables:
function bestPractices() {
    // 1. Use typeof for optional globals
    const maybeUndefined = typeof someGlobal !== 'undefined' ? someGlobal : 'default';
    
    // 2. Use try/catch for potential missing variables
    function safeAccess() {
        try {
            return someVariable;
        } catch (e) {
            return 'fallback value';
        }
    }
    
    // 3. Use optional chaining for object properties
    const obj = { nested: { value: 'exists' } };
    console.log(obj.nested?.value);   // 'exists'
    console.log(obj.missing?.value);  // undefined (no error)
}

bestPractices();
```
</details>

**Intermediate: Q1** - How does the scope chain relate to lexical scoping?

<details>
<summary>Answer</summary>

The scope chain implements lexical scoping by creating a linked structure of environments based on where code is written, not where it's executed:

```javascript
// LEXICAL SCOPING = Scope determined by code structure (where written)
// SCOPE CHAIN = Mechanism that implements lexical scoping

const globalVar = 'global';

function outerFunction() {
    const outerVar = 'outer';
    
    function innerFunction() {
        const innerVar = 'inner';
        
        // Scope chain: inner → outer → global
        // This chain is determined LEXICALLY (by code structure)
        console.log(innerVar);  // Current lexical environment
        console.log(outerVar);  // Parent lexical environment
        console.log(globalVar); // Global lexical environment
    }
    
    return innerFunction; // Function maintains its lexical scope chain
}

const returnedFunction = outerFunction();

// Even when called from different context, maintains original scope chain
function callInDifferentContext() {
    const contextVar = 'different context';
    
    // returnedFunction still sees its lexical scope chain
    returnedFunction(); // Accesses outerVar, not contextVar
}

callInDifferentContext();

// Lexical vs Dynamic scoping comparison:
const scopeVariable = 'global scope';

function lexicalScopeDemo() {
    const scopeVariable = 'function scope';
    
    function inner() {
        // LEXICAL: Looks where function was DEFINED
        console.log(scopeVariable); // 'function scope'
    }
    
    return inner;
}

function dynamicScopeSimulation(func) {
    const scopeVariable = 'call-site scope';
    
    // If JavaScript used dynamic scoping (it doesn't):
    // func() would see 'call-site scope'
    // But with lexical scoping, it sees 'function scope'
    
    func(); // Still logs 'function scope'
}

const lexicalFunction = lexicalScopeDemo();
dynamicScopeSimulation(lexicalFunction);

// Scope chain creation at definition time:
function createScopeChainExample() {
    const definitionTimeVar = 'defined here';
    
    // Scope chain established when function is DEFINED
    function definedFunction() {
        console.log(definitionTimeVar);
    }
    
    // Function maintains scope chain even when moved
    setTimeout(definedFunction, 100); // Still accesses definitionTimeVar
    
    return definedFunction;
}

const scopeChainFunction = createScopeChainExample();

// Module pattern with lexical scope:
const moduleWithLexicalScope = (function() {
    const privateVar = 'private to module';
    let moduleState = 0;
    
    return {
        increment() {
            moduleState++; // Accesses captured scope
            console.log('Module state:', moduleState);
        },
        
        getPrivate() {
            return privateVar; // Accesses captured scope
        }
    };
})();

moduleWithLexicalScope.increment(); // 1
```
</details>

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

<details>
<summary>Answer</summary>

The output will be `'outer'` due to lexical scoping and scope chain preservation:

```javascript
// Original code analysis:
var x = 'global';
function outer() {
    var x = 'outer';
    function inner() {
        console.log(x); // Logs 'outer'
    }
    return inner;
}
outer()(); // Output: 'outer'

// Step-by-step explanation:
// 1. Global variable x = 'global'
// 2. Function outer() creates local variable x = 'outer' (shadows global)
// 3. Function inner() is defined inside outer() - captures outer's scope
// 4. inner() returns, but maintains reference to outer's lexical environment
// 5. When inner() executes, it finds x in outer's scope: 'outer'

// Lexical environment preservation:
function demonstratePreservation() {
    var preservedVar = 'preserved value';
    
    function createClosure() {
        var closureVar = 'closure value';
        
        return function() {
            // Both outer scopes preserved
            console.log(preservedVar); // 'preserved value'
            console.log(closureVar);   // 'closure value'
        };
    }
    
    const closure = createClosure();
    // createClosure execution context is gone, but variables preserved
    
    closure(); // Still has access to both variables
}

demonstratePreservation();

// Comparison with different scenarios:

// Scenario 1: No variable shadowing
var scenario1 = 'global';
function noShadowing() {
    // No local variable declared
    function inner() {
        console.log(scenario1); // 'global'
    }
    return inner;
}
noShadowing()();

// Scenario 2: Multiple levels of shadowing
var scenario2 = 'global';
function multipleShadowing() {
    var scenario2 = 'outer';
    
    function middle() {
        var scenario2 = 'middle';
        
        function inner() {
            console.log(scenario2); // 'middle'
        }
        
        return inner;
    }
    
    return middle();
}
multipleShadowing()();

// Key concepts:
// 1. Lexical scoping determines variable access
// 2. Closures preserve scope chain
// 3. Variable shadowing hides outer variables
// 4. Scope resolution stops at first match
// 5. inner() accesses x from outer() scope via closure

console.log('Original code output: outer');
```
</details>

## Garbage collection

**Beginner: Q1** - What is garbage collection in JavaScript?

<details>
<summary>Answer</summary>

Garbage collection is an automatic memory management process that frees up memory by removing objects that are no longer reachable or referenced:

```javascript
// GARBAGE COLLECTION = Automatic memory management
// Removes objects that are no longer accessible

function demonstrateGarbageCollection() {
    // Object created in memory
    let obj = {
        name: 'John',
        age: 30,
        data: new Array(1000).fill('large data')
    };
    
    // Object is reachable via 'obj' variable
    console.log(obj.name); // Accessible
    
    // Remove reference - object becomes unreachable
    obj = null;
    
    // Now original object is eligible for garbage collection
    // (Will be cleaned up automatically by JavaScript engine)
}

demonstrateGarbageCollection();

// What makes objects eligible for GC:

// 1. No references
function noReferences() {
    let temp = { data: 'will be collected' };
    temp = null; // No more references
    // Object eligible for GC
}

// 2. Out of scope
function outOfScope() {
    {
        let scopedObj = { data: 'scoped' };
        // Object accessible here
    }
    // scopedObj out of scope - eligible for GC
}

// 3. Function execution complete
function functionComplete() {
    let localObj = { data: 'local' };
    return localObj.data; // Returns primitive, not object
    // localObj eligible for GC after function returns
}

// Reachability examples:
function reachabilityDemo() {
    let root = {
        child: {
            data: 'reachable via root'
        }
    };
    
    // Both root and child objects are reachable
    console.log(root.child.data);
    
    // Remove reference to child
    root.child = null;
    // Child object now unreachable - eligible for GC
    
    // Remove reference to root
    root = null;
    // Root object now unreachable - eligible for GC
}

reachabilityDemo();

// Memory lifecycle:
function memoryLifecycle() {
    // 1. ALLOCATION - Memory allocated when object created
    const allocated = new Object();
    
    // 2. USE - Object is used/accessed
    allocated.property = 'value';
    console.log(allocated.property);
    
    // 3. RELEASE - Object becomes unreachable
    return; // allocated goes out of scope
    
    // 4. COLLECTION - GC automatically frees memory
}

memoryLifecycle();
```
</details>

**Beginner: Q2** - When does garbage collection happen?

<details>
<summary>Answer</summary>

Garbage collection occurs automatically at intervals determined by the JavaScript engine, typically when memory usage reaches certain thresholds:

```javascript
// WHEN GC OCCURS:
// 1. Automatically by JavaScript engine
// 2. When memory usage increases
// 3. During idle time (engine-dependent)
// 4. When heap size reaches thresholds

// Triggering conditions (engine-dependent):
function gcTriggerConditions() {
    // 1. Memory pressure
    const largeArrays = [];
    for (let i = 0; i < 1000; i++) {
        largeArrays.push(new Array(10000).fill(`data ${i}`));
        // GC may trigger due to memory pressure
    }
    
    // 2. Allocation rate
    function highAllocationRate() {
        for (let i = 0; i < 100000; i++) {
            const temp = { id: i, data: `item ${i}` };
            // Rapid allocations may trigger GC
        }
    }
    
    highAllocationRate();
    
    // 3. Time intervals (engine-specific)
    setTimeout(() => {
        // GC may occur during idle periods
        console.log('Some time has passed - GC may have occurred');
    }, 1000);
}

// You cannot force garbage collection in standard JavaScript
// (Some engines provide methods like gc() in development)

// Monitoring GC (development tools):
function monitoringGC() {
    // Performance API can show memory usage
    if (performance.memory) {
        console.log('Used heap:', performance.memory.usedJSHeapSize);
        console.log('Total heap:', performance.memory.totalJSHeapSize);
        console.log('Heap limit:', performance.memory.jsHeapSizeLimit);
    }
    
    // Create objects to potentially trigger GC
    const objects = [];
    for (let i = 0; i < 50000; i++) {
        objects.push({ id: i, data: new Array(100).fill(i) });
    }
    
    // Check memory again
    setTimeout(() => {
        if (performance.memory) {
            console.log('After allocation:', performance.memory.usedJSHeapSize);
        }
        
        // Clear references
        objects.length = 0;
        
        // Memory may be freed in next GC cycle
        setTimeout(() => {
            if (performance.memory) {
                console.log('After clearing:', performance.memory.usedJSHeapSize);
            }
        }, 100);
    }, 100);
}

// Best practices for GC timing:
function gcBestPractices() {
    // 1. Don't try to control GC timing
    // 2. Focus on making objects eligible for collection
    // 3. Remove references when done
    
    let heavyResource = {
        data: new Array(100000).fill('heavy data'),
        cleanup() {
            this.data = null; // Help GC by clearing references
        }
    };
    
    // Use resource
    console.log('Resource size:', heavyResource.data.length);
    
    // Clean up when done
    heavyResource.cleanup();
    heavyResource = null; // Make object eligible for GC
}

gcBestPractices();

// Different GC patterns:
// - Generational GC: Young objects collected more frequently
// - Incremental GC: Small chunks to avoid blocking
// - Concurrent GC: GC runs alongside main thread
```
</details>

**Beginner: Q3** - What types of values are eligible for garbage collection?

<details>
<summary>Answer</summary>

Only reference types (objects, arrays, functions) are eligible for garbage collection. Primitive values are managed differently:

```javascript
// ELIGIBLE FOR GC: Reference types (objects)
// NOT ELIGIBLE: Primitive values (managed on stack/registers)

// Reference types eligible for GC:
function eligibleTypes() {
    // 1. Objects
    let obj = { name: 'John', age: 30 };
    obj = null; // Object eligible for GC
    
    // 2. Arrays
    let arr = [1, 2, 3, 4, 5];
    arr = null; // Array eligible for GC
    
    // 3. Functions
    let func = function() { return 'hello'; };
    func = null; // Function eligible for GC
    
    // 4. Dates
    let date = new Date();
    date = null; // Date object eligible for GC
    
    // 5. Regular expressions
    let regex = /pattern/g;
    regex = null; // RegExp object eligible for GC
    
    // 6. DOM elements (in browsers)
    // let element = document.createElement('div');
    // element = null; // Element eligible for GC
}

// Primitive types NOT eligible for GC:
function primitiveTypes() {
    // Primitives are stored on stack or in registers
    let num = 42;           // Number primitive
    let str = 'hello';      // String primitive
    let bool = true;        // Boolean primitive
    let undef = undefined;  // Undefined primitive
    let nul = null;         // Null primitive
    let sym = Symbol('id'); // Symbol primitive
    let big = 123n;         // BigInt primitive
    
    // These are not "collected" - they're just deallocated
    // when variables go out of scope
}

// Complex scenarios:
function complexScenarios() {
    // Objects containing primitives
    let container = {
        number: 42,        // Primitive stored in object
        string: 'hello',   // Primitive stored in object
        nested: {          // Nested object
            value: 100
        }
    };
    
    container = null; // Object and nested object eligible for GC
                     // Primitives inside are deallocated with object
    
    // Arrays containing mixed types
    let mixedArray = [
        42,                    // Primitive element
        'string',              // Primitive element
        { id: 1 },            // Object element
        [1, 2, 3],            // Array element
        function() { return 1; } // Function element
    ];
    
    mixedArray = null; // Array and all object elements eligible for GC
                       // Primitive elements deallocated with array
    
    // Functions with closures
    function createClosure() {
        let closureVar = { data: 'closure data' }; // Object in closure
        let primitiveVar = 'primitive in closure';  // Primitive in closure
        
        return function() {
            console.log(closureVar.data);   // Keeps object alive
            console.log(primitiveVar);      // Primitive captured in closure
        };
    }
    
    let closure = createClosure();
    // closureVar object kept alive by closure
    closure = null; // Now closure and captured object eligible for GC
}

// What happens to each type:
function typeManagement() {
    // PRIMITIVES: Stored in variables/stack
    // - Automatically freed when scope ends
    // - No need for garbage collection
    
    // OBJECTS: Stored in heap
    // - Referenced by variables/properties
    // - Eligible for GC when unreachable
    // - Actually freed during GC cycles
    
    console.log('Primitives: Stack/register management');
    console.log('Objects: Heap allocation + garbage collection');
}

eligibleTypes();
complexScenarios();
typeManagement();
```
</details>

**Intermediate: Q1** - What is the mark-and-sweep algorithm?

<details>
<summary>Answer</summary>

Mark-and-sweep is the most common garbage collection algorithm that works in two phases: marking reachable objects and sweeping (freeing) unmarked objects:

```javascript
// MARK-AND-SWEEP ALGORITHM:
// PHASE 1: MARK - Traverse from roots, mark reachable objects
// PHASE 2: SWEEP - Free all unmarked objects

// Simulating mark-and-sweep concept:
function markAndSweepSimulation() {
    // PHASE 1: MARK - Starting from roots
    
    // ROOTS (always reachable):
    // - Global variables
    // - Local variables in call stack
    // - DOM elements in use
    
    let root1 = {           // ROOT: Global variable
        id: 'root1',
        child: {
            id: 'child1',
            data: 'reachable'
        }
    };
    
    let root2 = {           // ROOT: Global variable
        id: 'root2',
        reference: root1    // Points to root1
    };
    
    // Orphaned object (not reachable from roots)
    let orphan = {
        id: 'orphan',
        data: 'unreachable'
    };
    
    // Remove reference to orphan
    orphan = null;
    
    // MARKING PROCESS (conceptual):
    /*
    1. Start from root1 (MARKED)
       ├── child object (MARKED via root1.child)
    
    2. Start from root2 (MARKED)
       ├── root1 object (ALREADY MARKED)
           ├── child object (ALREADY MARKED)
    
    3. orphan object (UNMARKED - no path from roots)
    */
    
    // SWEEPING PROCESS:
    // - All UNMARKED objects are freed
    // - orphan object would be collected
    
    console.log('Mark-and-sweep would collect orphaned objects');
}

// Detailed algorithm steps:
function markAndSweepSteps() {
    // Step 1: Initialize - All objects unmarked
    const objects = [
        { id: 'obj1', marked: false, refs: ['obj2'] },
        { id: 'obj2', marked: false, refs: [] },
        { id: 'obj3', marked: false, refs: ['obj1'] },
        { id: 'obj4', marked: false, refs: [] }  // Orphaned
    ];
    
    const roots = ['obj1', 'obj3']; // Reachable from roots
    
    // Step 2: MARK phase - Depth-first traversal
    function markReachable(objId, visited = new Set()) {
        if (visited.has(objId)) return;
        visited.add(objId);
        
        const obj = objects.find(o => o.id === objId);
        if (obj) {
            obj.marked = true;
            console.log(`Marked: ${objId}`);
            
            // Mark all referenced objects
            obj.refs.forEach(ref => markReachable(ref, visited));
        }
    }
    
    // Mark from all roots
    roots.forEach(root => markReachable(root));
    
    // Step 3: SWEEP phase - Free unmarked objects
    console.log('\nSweep phase:');
    objects.forEach(obj => {
        if (!obj.marked) {
            console.log(`Collecting: ${obj.id}`);
        } else {
            console.log(`Keeping: ${obj.id}`);
        }
    });
}

markAndSweepSteps();

// Real-world mark-and-sweep behavior:
function realWorldBehavior() {
    // Objects that reference each other
    let objA = { id: 'A' };
    let objB = { id: 'B' };
    
    // Create circular reference
    objA.ref = objB;
    objB.ref = objA;
    
    // Both objects reachable from variables (roots)
    console.log('Both objects reachable');
    
    // Remove root references
    objA = null;
    objB = null;
    
    // Now both objects are unreachable despite circular reference
    // Mark-and-sweep will collect both (no path from roots)
    console.log('Circular reference will be collected');
    
    // Complex object graph
    function complexGraph() {
        let root = {
            id: 'root',
            children: [
                {
                    id: 'child1',
                    parent: null, // Will be set to root
                    data: new Array(1000).fill('data')
                },
                {
                    id: 'child2',
                    parent: null, // Will be set to root
                    siblings: []
                }
            ]
        };
        
        // Create parent references
        root.children[0].parent = root;
        root.children[1].parent = root;
        
        // Create sibling references
        root.children[0].siblings = [root.children[1]];
        root.children[1].siblings = [root.children[0]];
        
        // Complex graph with multiple references
        // All reachable from 'root' variable
        
        return root;
    }
    
    let complexObj = complexGraph();
    // All objects in graph are reachable
    
    complexObj = null;
    // Entire graph becomes unreachable - all will be collected
}

realWorldBehavior();

// Advantages of mark-and-sweep:
console.log(`
Mark-and-sweep advantages:
1. Handles circular references
2. Precise (collects exactly unreachable objects)
3. No reference counting overhead
4. Works with complex object graphs
`);
```
</details>

**Intermediate: Q2** - How can circular references cause memory leaks in older JavaScript engines?

<details>
<summary>Answer</summary>

Older JavaScript engines used reference counting which couldn't handle circular references, causing memory leaks. Modern engines use mark-and-sweep which solves this:

```javascript
// HISTORICAL PROBLEM: Reference counting + Circular references = Memory leaks
// MODERN SOLUTION: Mark-and-sweep correctly handles circular references

// Reference counting algorithm (OLD):
function referenceCountingProblem() {
    // OLD ALGORITHM:
    // 1. Each object has a reference count
    // 2. Count increments when referenced
    // 3. Count decrements when reference removed
    // 4. Object freed when count reaches 0
    
    // PROBLEM: Circular references never reach count 0
    
    function createCircularLeak() {
        let objA = { name: 'Object A', refCount: 0 };
        let objB = { name: 'Object B', refCount: 0 };
        
        // Create circular reference
        objA.ref = objB;    // objB.refCount = 1
        objB.ref = objA;    // objA.refCount = 1
        
        // Even when variables go out of scope:
        // objA.refCount = 1 (referenced by objB)
        // objB.refCount = 1 (referenced by objA)
        // Neither object is freed = MEMORY LEAK
        
        return { objA, objB };
    }
    
    let result = createCircularLeak();
    result = null; // Variables cleared, but objects still reference each other
    
    console.log('With reference counting: MEMORY LEAK');
}

// Real-world memory leak scenarios (historical):
function historicalLeakScenarios() {
    // 1. DOM element circular references (old IE)
    function domCircularLeak() {
        // This would cause leaks in old browsers:
        /*
        let element = document.getElementById('myDiv');
        let data = {
            element: element,
            info: 'some data'
        };
        element.customData = data; // Circular reference
        
        // When element removed from DOM:
        // - element still references data
        // - data still references element
        // = MEMORY LEAK in old IE
        */
        
        console.log('DOM circular references caused leaks in old IE');
    }
    
    // 2. Event handler circular references
    function eventHandlerLeak() {
        /*
        let button = document.createElement('button');
        let handler = {
            element: button,
            handleClick: function() {
                console.log('Clicked');
            }
        };
        
        button.addEventListener('click', handler.handleClick);
        button.customHandler = handler; // Circular reference
        
        // handler references button via element property
        // button references handler via customHandler property
        // = MEMORY LEAK in old browsers
        */
        
        console.log('Event handler circular references caused leaks');
    }
    
    domCircularLeak();
    eventHandlerLeak();
}

// Modern mark-and-sweep solution:
function modernSolution() {
    // MARK-AND-SWEEP ALGORITHM:
    // 1. Start from "roots" (global variables, DOM, etc.)
    // 2. Mark all reachable objects
    // 3. Sweep (free) all unmarked objects
    // 4. Circular references don't matter if unreachable from roots
    
    function circularReferenceSolved() {
        let objA = { name: 'A' };
        let objB = { name: 'B' };
        
        // Create circular reference
        objA.ref = objB;
        objB.ref = objA;
        
        // Objects are reachable from variables (roots)
        console.log('Objects reachable from roots');
        
        // Remove root references
        objA = null;
        objB = null;
        
        // Mark-and-sweep analysis:
        // 1. MARK phase: Start from roots (objA=null, objB=null)
        // 2. No path to either object from roots
        // 3. Both objects remain UNMARKED
        // 4. SWEEP phase: Both objects are freed
        
        console.log('Mark-and-sweep: Circular reference cleaned up! ✅');
    }
    
    circularReferenceSolved();
}

// Comparison of algorithms:
function algorithmComparison() {
    const scenarios = [
        {
            name: 'Simple object',
            leak: 'None',
            refCounting: 'Works ✅',
            markSweep: 'Works ✅'
        },
        {
            name: 'Circular reference',
            leak: 'Memory leak',
            refCounting: 'Fails ❌',
            markSweep: 'Works ✅'
        },
        {
            name: 'Complex graph',
            leak: 'Partial leaks',
            refCounting: 'Partial ❌',
            markSweep: 'Works ✅'
        }
    ];
    
    console.table(scenarios);
}

// Best practices for avoiding leaks:
function bestPractices() {
    // Even with modern GC, good practices help:
    
    // 1. Explicitly null references when done
    function explicitCleanup() {
        let heavyObject = { data: new Array(100000).fill('data') };
        
        // Use object...
        console.log('Using heavy object');
        
        // Explicitly clear when done
        heavyObject = null; // Help GC
    }
    
    // 2. Use WeakMap for auxiliary references
    function useWeakReferences() {
        const metadata = new WeakMap();
        
        let obj = { id: 1 };
        metadata.set(obj, { created: new Date() });
        
        // When obj is freed, metadata is automatically cleaned
        obj = null; // No circular reference with WeakMap
    }
    
    // 3. Remove event listeners
    function cleanupEventListeners() {
        let button = document.createElement('button');
        let controller = new AbortController();
        
        button.addEventListener('click', () => {
            console.log('clicked');
        }, { signal: controller.signal });
        
        // Clean up properly
        controller.abort(); // Removes all listeners
        button = null;
    }
    
    explicitCleanup();
    useWeakReferences();
}

referenceCountingProblem();
historicalLeakScenarios();
modernSolution();
algorithmComparison();
bestPractices();

console.log(`
Summary:
- Old engines: Reference counting → Circular reference leaks
- Modern engines: Mark-and-sweep → Handles circular references
- Best practice: Focus on making objects unreachable from roots
`);
```
</details>

## Memoization

**Beginner: Q1** - What is memoization?

<details>
<summary>Answer</summary>

Memoization is an optimization technique that caches the results of expensive function calls and returns the cached result for the same inputs:

```javascript
// MEMOIZATION = Cache function results to avoid repeated computation
// Key concept: Cache input → output mapping

// Without memoization (expensive repeated calculations):
function expensiveFunction(n) {
    console.log(`Computing for ${n}...`);
    
    // Simulate expensive computation
    let result = 0;
    for (let i = 0; i < n; i++) {
        result += Math.sqrt(i);
    }
    
    return result;
}

// Multiple calls do same work:
console.log(expensiveFunction(1000000)); // Computes
console.log(expensiveFunction(1000000)); // Computes again (wasteful)

// With memoization:
function createMemoizedFunction() {
    const cache = new Map(); // Store results
    
    return function memoizedExpensive(n) {
        // Check if result already cached
        if (cache.has(n)) {
            console.log(`Cache hit for ${n}`);
            return cache.get(n);
        }
        
        // Compute and cache result
        console.log(`Computing for ${n}...`);
        let result = 0;
        for (let i = 0; i < n; i++) {
            result += Math.sqrt(i);
        }
        
        cache.set(n, result);
        return result;
    };
}

const memoizedFunction = createMemoizedFunction();

console.log(memoizedFunction(1000000)); // Computes
console.log(memoizedFunction(1000000)); // Cache hit! ✅

// Simple memoization example:
function memoize(fn) {
    const cache = {};
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (key in cache) {
            return cache[key];
        }
        
        const result = fn.apply(this, args);
        cache[key] = result;
        return result;
    };
}

// Usage:
const slowAdd = (a, b) => {
    console.log('Computing sum...');
    return a + b;
};

const fastAdd = memoize(slowAdd);

console.log(fastAdd(1, 2)); // Computing sum... 3
console.log(fastAdd(1, 2)); // 3 (cached)

// Fibonacci with and without memoization:
function fibonacciSlow(n) {
    if (n <= 1) return n;
    return fibonacciSlow(n - 1) + fibonacciSlow(n - 2);
}

const fibonacciFast = memoize(function(n) {
    if (n <= 1) return n;
    return fibonacciFast(n - 1) + fibonacciFast(n - 2);
});

console.log('Slow:', fibonacciSlow(10)); // Many redundant calculations
console.log('Fast:', fibonacciFast(10)); // Cached intermediate results
```
</details>

**Beginner: Q2** - When would you use memoization?

<details>
<summary>Answer</summary>

Use memoization for pure functions with expensive computations that are called repeatedly with the same inputs:

```javascript
// IDEAL CANDIDATES FOR MEMOIZATION:
// 1. Pure functions (same input → same output)
// 2. Expensive computations
// 3. Frequently called with same inputs
// 4. No side effects

// 1. Mathematical calculations:
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

const memoizedFactorial = memoize(factorial);

// Use case: Computing factorials in combinatorics
function combinations(n, r) {
    return memoizedFactorial(n) / (memoizedFactorial(r) * memoizedFactorial(n - r));
}

// 2. Recursive algorithms:
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

const memoizedFib = memoize(fibonacci);

// Without memoization: O(2^n) - exponential
// With memoization: O(n) - linear

// 3. API calls with same parameters:
async function fetchUserData(userId) {
    console.log(`Fetching user ${userId}...`);
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
}

const memoizedFetch = memoize(fetchUserData);

// Multiple calls to same user ID use cached result
async function getUserProfile(userId) {
    const userData = await memoizedFetch(userId); // Cache hit on repeat calls
    return userData;
}

// 4. Complex data transformations:
function transformLargeDataset(data) {
    console.log('Transforming dataset...');
    return data
        .filter(item => item.active)
        .map(item => ({
            ...item,
            computed: item.value * 2.5,
            formatted: item.date.toISOString()
        }))
        .sort((a, b) => a.computed - b.computed);
}

const memoizedTransform = memoize(transformLargeDataset);

// 5. React component rendering optimization:
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
    const processedData = useMemo(() => {
        return data.map(item => ({
            ...item,
            expensive: expensiveCalculation(item)
        }));
    }, [data]); // Only recalculate when data changes
    
    return <div>{processedData.map(item => <Item key={item.id} {...item} />)}</div>;
});

// WHEN NOT TO USE MEMOIZATION:

// 1. Functions with side effects:
function impureFunction(x) {
    console.log('Side effect!'); // Side effect
    window.someGlobal = x;       // Side effect
    return x * 2;
}
// Don't memoize - side effects should execute every time

// 2. Functions that return different results for same input:
function randomFunction(x) {
    return x + Math.random(); // Different result each time
}
// Don't memoize - violates pure function requirement

// 3. Simple, fast computations:
function simpleAdd(a, b) {
    return a + b; // Too simple to benefit from memoization
}
// Don't memoize - overhead > benefit

// 4. Functions called infrequently:
function rarelyCalledFunction(x) {
    return expensiveCalculation(x);
}
// Don't memoize if called once per session

// Memory considerations:
function memoryAwareMemoization() {
    // Problem: Unbounded cache growth
    const cache = new Map();
    
    // Solution: LRU (Least Recently Used) cache
    function createLRUMemoize(maxSize) {
        const cache = new Map();
        
        return function(fn) {
            return function(...args) {
                const key = JSON.stringify(args);
                
                if (cache.has(key)) {
                    // Move to end (mark as recently used)
                    const value = cache.get(key);
                    cache.delete(key);
                    cache.set(key, value);
                    return value;
                }
                
                // Remove oldest if at capacity
                if (cache.size >= maxSize) {
                    const firstKey = cache.keys().next().value;
                    cache.delete(firstKey);
                }
                
                const result = fn.apply(this, args);
                cache.set(key, result);
                return result;
            };
        };
    }
    
    const lruMemoize = createLRUMemoize(100); // Max 100 cached results
    const memoizedFn = lruMemoize(expensiveFunction);
}

function memoize(fn) {
    const cache = {};
    return function(...args) {
        const key = JSON.stringify(args);
        if (key in cache) return cache[key];
        const result = fn.apply(this, args);
        cache[key] = result;
        return result;
    };
}

function expensiveCalculation(x) {
    return x * x * x;
}

memoryAwareMemoization();
```
</details>

**Beginner: Q3** - Give a simple example of memoization.

<details>
<summary>Answer</summary>

Here's a simple memoization example using a square calculation function:

```javascript
// Simple memoization example: Calculating squares

// Basic function without memoization:
function calculateSquare(n) {
    console.log(`Calculating square of ${n}`);
    return n * n;
}

// Without memoization - repeats work:
console.log(calculateSquare(5)); // Calculating square of 5 → 25
console.log(calculateSquare(5)); // Calculating square of 5 → 25 (calculated again)

// WITH MEMOIZATION:
function createMemoizedSquare() {
    const cache = {}; // Store previous results
    
    return function(n) {
        // Check if we already calculated this
        if (n in cache) {
            console.log(`Cache hit for ${n}`);
            return cache[n];
        }
        
        // Calculate and store result
        console.log(`Calculating square of ${n}`);
        const result = n * n;
        cache[n] = result;
        
        return result;
    };
}

const memoizedSquare = createMemoizedSquare();

// Now with memoization:
console.log(memoizedSquare(5)); // Calculating square of 5 → 25
console.log(memoizedSquare(5)); // Cache hit for 5 → 25 (no calculation!)
console.log(memoizedSquare(3)); // Calculating square of 3 → 9
console.log(memoizedSquare(5)); // Cache hit for 5 → 25
console.log(memoizedSquare(3)); // Cache hit for 3 → 9

// Generic memoization function:
function memoize(fn) {
    const cache = {};
    
    return function(...args) {
        // Create unique key from arguments
        const key = JSON.stringify(args);
        
        // Return cached result if exists
        if (key in cache) {
            console.log('Cache hit!');
            return cache[key];
        }
        
        // Calculate and cache result
        console.log('Computing...');
        const result = fn.apply(this, args);
        cache[key] = result;
        
        return result;
    };
}

// Usage with different functions:

// 1. Simple addition:
const add = (a, b) => a + b;
const memoizedAdd = memoize(add);

console.log(memoizedAdd(2, 3)); // Computing... → 5
console.log(memoizedAdd(2, 3)); // Cache hit! → 5

// 2. String manipulation:
const reverseString = (str) => str.split('').reverse().join('');
const memoizedReverse = memoize(reverseString);

console.log(memoizedReverse('hello')); // Computing... → 'olleh'
console.log(memoizedReverse('hello')); // Cache hit! → 'olleh'

// 3. Object transformation:
const processUser = (user) => ({
    ...user,
    fullName: `${user.firstName} ${user.lastName}`,
    initials: `${user.firstName[0]}${user.lastName[0]}`
});

const memoizedProcessUser = memoize(processUser);

const user = { firstName: 'John', lastName: 'Doe' };

console.log(memoizedProcessUser(user)); // Computing... → processed user
console.log(memoizedProcessUser(user)); // Cache hit! → processed user

// Visual representation of cache:
function demonstrateCache() {
    const cache = {};
    
    function memoizedFunction(x) {
        const key = String(x);
        
        if (key in cache) {
            console.log(`Cache: {${Object.keys(cache).join(', ')}} - HIT for ${x}`);
            return cache[key];
        }
        
        const result = x * x;
        cache[key] = result;
        console.log(`Cache: {${Object.keys(cache).join(', ')}} - MISS for ${x}, computed ${result}`);
        
        return result;
    }
    
    memoizedFunction(2); // Cache: {2} - MISS for 2, computed 4
    memoizedFunction(3); // Cache: {2, 3} - MISS for 3, computed 9
    memoizedFunction(2); // Cache: {2, 3} - HIT for 2
    memoizedFunction(4); // Cache: {2, 3, 4} - MISS for 4, computed 16
    memoizedFunction(3); // Cache: {2, 3, 4} - HIT for 3
}

demonstrateCache();

// Performance comparison:
function performanceComparison() {
    function slowFunction(n) {
        // Simulate slow computation
        let result = 0;
        for (let i = 0; i < n * 1000000; i++) {
            result += i;
        }
        return result;
    }
    
    const memoizedSlow = memoize(slowFunction);
    
    console.time('First call (no cache)');
    memoizedSlow(100);
    console.timeEnd('First call (no cache)');
    
    console.time('Second call (cached)');
    memoizedSlow(100);
    console.timeEnd('Second call (cached)');
}

performanceComparison();
```
</details>

**Intermediate: Q1** - Implement a generic memoization function.

<details>
<summary>Answer</summary>

Here's a comprehensive generic memoization function with advanced features:

```javascript
// BASIC GENERIC MEMOIZATION:
function basicMemoize(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

// ADVANCED GENERIC MEMOIZATION with options:
function advancedMemoize(fn, options = {}) {
    const {
        maxSize = Infinity,           // Maximum cache size
        ttl = Infinity,              // Time to live (milliseconds)
        keyGenerator = JSON.stringify, // Custom key generation
        onCacheHit,                  // Callback for cache hits
        onCacheMiss,                 // Callback for cache misses
        onEviction                   // Callback for evictions
    } = options;
    
    const cache = new Map();
    const timestamps = new Map(); // For TTL tracking
    
    function evictExpired() {
        const now = Date.now();
        for (const [key, timestamp] of timestamps) {
            if (now - timestamp > ttl) {
                const evictedValue = cache.get(key);
                cache.delete(key);
                timestamps.delete(key);
                onEviction?.(key, evictedValue, 'expired');
            }
        }
    }
    
    function evictOldest() {
        const oldestKey = cache.keys().next().value;
        const evictedValue = cache.get(oldestKey);
        cache.delete(oldestKey);
        timestamps.delete(oldestKey);
        onEviction?.(oldestKey, evictedValue, 'size-limit');
    }
    
    const memoizedFn = function(...args) {
        // Generate cache key
        const key = keyGenerator(args);
        
        // Clean up expired entries
        if (ttl < Infinity) {
            evictExpired();
        }
        
        // Check cache
        if (cache.has(key)) {
            // Update timestamp for LRU
            const value = cache.get(key);
            cache.delete(key);
            cache.set(key, value);
            timestamps.set(key, Date.now());
            
            onCacheHit?.(key, value);
            return value;
        }
        
        // Cache miss - compute result
        const result = fn.apply(this, args);
        
        // Evict oldest if at capacity
        if (cache.size >= maxSize) {
            evictOldest();
        }
        
        // Store result
        cache.set(key, result);
        timestamps.set(key, Date.now());
        
        onCacheMiss?.(key, result);
        return result;
    };
    
    // Add utility methods
    memoizedFn.cache = cache;
    memoizedFn.clear = () => {
        cache.clear();
        timestamps.clear();
    };
    memoizedFn.delete = (key) => {
        cache.delete(key);
        timestamps.delete(key);
    };
    memoizedFn.has = (key) => cache.has(key);
    memoizedFn.size = () => cache.size;
    
    return memoizedFn;
}

// CUSTOM KEY GENERATORS:
const keyGenerators = {
    // Default JSON stringify
    json: JSON.stringify,
    
    // Simple string concatenation
    concat: (args) => args.join('|'),
    
    // Hash-based key (for objects)
    hash: (args) => {
        return args.map(arg => {
            if (typeof arg === 'object') {
                return Object.keys(arg).sort().map(k => `${k}:${arg[k]}`).join(',');
            }
            return String(arg);
        }).join('|');
    },
    
    // First argument only
    firstArg: (args) => String(args[0]),
    
    // Custom object serialization
    deepSerialize: (args) => {
        return JSON.stringify(args, (key, value) => {
            if (typeof value === 'function') return '[Function]';
            if (value instanceof Date) return `[Date:${value.toISOString()}]`;
            if (value instanceof RegExp) return `[RegExp:${value.toString()}]`;
            return value;
        });
    }
};

// USAGE EXAMPLES:

// 1. Basic usage:
const expensiveFunction = (n) => {
    console.log(`Computing for ${n}`);
    return n * n * n;
};

const memoized = basicMemoize(expensiveFunction);
console.log(memoized(5)); // Computing for 5 → 125
console.log(memoized(5)); // 125 (cached)

// 2. With size limit:
const limitedMemoize = advancedMemoize(expensiveFunction, {
    maxSize: 3,
    onEviction: (key, value, reason) => {
        console.log(`Evicted ${key}=${value} (${reason})`);
    }
});

limitedMemoize(1); // Computing for 1
limitedMemoize(2); // Computing for 2  
limitedMemoize(3); // Computing for 3
limitedMemoize(4); // Computing for 4, Evicted [1]=1 (size-limit)

// 3. With TTL (Time To Live):
const ttlMemoize = advancedMemoize(expensiveFunction, {
    ttl: 1000, // 1 second
    onEviction: (key, value, reason) => {
        console.log(`Evicted ${key}=${value} (${reason})`);
    }
});

ttlMemoize(10); // Computing for 10
setTimeout(() => {
    ttlMemoize(10); // Computing for 10 (expired)
}, 1100);

// 4. With custom key generator:
const customKeyMemoize = advancedMemoize(
    (obj) => obj.a + obj.b,
    {
        keyGenerator: keyGenerators.hash,
        onCacheHit: (key) => console.log(`Cache hit: ${key}`),
        onCacheMiss: (key) => console.log(`Cache miss: ${key}`)
    }
);

customKeyMemoize({ a: 1, b: 2 }); // Cache miss: a:1,b:2 → 3
customKeyMemoize({ b: 2, a: 1 }); // Cache hit: a:1,b:2 → 3 (same object)

// 5. Async function memoization:
function memoizeAsync(asyncFn, options = {}) {
    const cache = new Map();
    
    return async function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const promise = asyncFn.apply(this, args);
        cache.set(key, promise);
        
        try {
            const result = await promise;
            return result;
        } catch (error) {
            // Remove failed promise from cache
            cache.delete(key);
            throw error;
        }
    };
}

// Usage with async functions:
const fetchData = async (id) => {
    console.log(`Fetching data for ${id}`);
    const response = await fetch(`/api/data/${id}`);
    return response.json();
};

const memoizedFetch = memoizeAsync(fetchData);

// 6. Class method memoization:
class Calculator {
    constructor() {
        this.compute = advancedMemoize(this.compute.bind(this));
    }
    
    compute(n) {
        console.log(`Computing ${n}!`);
        let result = 1;
        for (let i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }
}

const calc = new Calculator();
console.log(calc.compute(5)); // Computing 5! → 120
console.log(calc.compute(5)); // 120 (cached)

// Utility function for React components:
function memoizeComponent(Component) {
    return React.memo(Component, (prevProps, nextProps) => {
        return JSON.stringify(prevProps) === JSON.stringify(nextProps);
    });
}
```
</details>

**Intermediate: Q2** - What are the trade-offs of using memoization?

<details>
<summary>Answer</summary>

Memoization has important trade-offs between performance gains and memory/complexity costs:

```javascript
// MEMOIZATION TRADE-OFFS ANALYSIS

// ✅ BENEFITS:
console.log('=== BENEFITS ===');

// 1. Performance improvement for expensive repeated calculations
function demonstratePerformanceBenefit() {
    function fibonacci(n) {
        if (n <= 1) return n;
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
    
    const memoizedFib = memoize(fibonacci);
    
    console.time('Without memoization (fib 35)');
    fibonacci(35); // ~29 million function calls
    console.timeEnd('Without memoization (fib 35)');
    
    console.time('With memoization (fib 35)');
    memoizedFib(35); // ~35 function calls
    console.timeEnd('With memoization (fib 35)');
    
    // Time complexity: O(2^n) → O(n)
}

// ❌ COSTS:
console.log('\n=== COSTS ===');

// 1. Memory overhead - storing cached results
function memoryOverhead() {
    const cache = new Map();
    
    function expensiveFunction(data) {
        const key = JSON.stringify(data);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = data.map(x => x * x);
        cache.set(key, result); // Memory grows with each unique input
        return result;
    }
    
    // Each call with different input increases memory usage
    const largeArray = new Array(10000).fill(0).map((_, i) => i);
    
    for (let i = 0; i < 100; i++) {
        expensiveFunction(largeArray.slice(0, i * 100));
    }
    
    console.log(`Cache size: ${cache.size} entries`);
    console.log('Memory grows linearly with unique inputs');
}

// 2. Key generation overhead
function keyGenerationOverhead() {
    function complexObject() {
        return {
            id: Math.random(),
            data: new Array(1000).fill('data'),
            nested: { deep: { object: { structure: true } } }
        };
    }
    
    const obj = complexObject();
    
    console.time('JSON.stringify overhead');
    for (let i = 0; i < 10000; i++) {
        JSON.stringify(obj); // Expensive for large objects
    }
    console.timeEnd('JSON.stringify overhead');
    
    console.log('Key generation can be expensive for complex objects');
}

// 3. Memory leaks in long-running applications
function memoryLeakRisk() {
    const cache = new Map();
    
    function userProcessor(userData) {
        const key = userData.id;
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const processed = processUserData(userData);
        cache.set(key, processed); // Never cleared!
        return processed;
    }
    
    // In a long-running app, cache grows indefinitely
    // Solution: Implement cache eviction strategies
    
    console.log('Unbounded caches can cause memory leaks');
}

// WHEN MEMOIZATION HURTS PERFORMANCE:
console.log('\n=== WHEN MEMOIZATION HURTS ===');

// 1. Frequently changing inputs
function changingInputs() {
    let counter = 0;
    
    function alwaysChanging() {
        return { timestamp: Date.now(), random: Math.random(), counter: counter++ };
    }
    
    const memoizedChanging = memoize(alwaysChanging);
    
    // Cache never hits - only overhead
    console.time('Without memoization');
    for (let i = 0; i < 10000; i++) {
        alwaysChanging();
    }
    console.timeEnd('Without memoization');
    
    console.time('With memoization (never hits)');
    for (let i = 0; i < 10000; i++) {
        memoizedChanging();
    }
    console.timeEnd('With memoization (never hits)');
    
    console.log('Memoization adds overhead when cache never hits');
}

// 2. Simple computations
function simpleComputations() {
    function simpleAdd(a, b) {
        return a + b;
    }
    
    const memoizedAdd = memoize(simpleAdd);
    
    console.time('Simple function direct');
    for (let i = 0; i < 1000000; i++) {
        simpleAdd(i, i + 1);
    }
    console.timeEnd('Simple function direct');
    
    console.time('Simple function memoized');
    for (let i = 0; i < 1000000; i++) {
        memoizedAdd(i, i + 1);
    }
    console.timeEnd('Simple function memoized');
    
    console.log('Memoization overhead > computation cost for simple functions');
}

// SOLUTIONS TO TRADE-OFFS:
console.log('\n=== SOLUTIONS ===');

// 1. LRU (Least Recently Used) cache
function lruSolution() {
    function createLRUMemoize(maxSize) {
        const cache = new Map();
        
        return function(fn) {
            return function(...args) {
                const key = JSON.stringify(args);
                
                if (cache.has(key)) {
                    // Move to end (most recently used)
                    const value = cache.get(key);
                    cache.delete(key);
                    cache.set(key, value);
                    return value;
                }
                
                // Remove oldest if at capacity
                if (cache.size >= maxSize) {
                    const firstKey = cache.keys().next().value;
                    cache.delete(firstKey);
                }
                
                const result = fn.apply(this, args);
                cache.set(key, result);
                return result;
            };
        };
    }
    
    const lruMemoize = createLRUMemoize(100);
    console.log('LRU cache prevents unbounded memory growth');
}

// 2. TTL (Time To Live) cache
function ttlSolution() {
    function createTTLMemoize(ttl) {
        const cache = new Map();
        const timestamps = new Map();
        
        return function(fn) {
            return function(...args) {
                const key = JSON.stringify(args);
                const now = Date.now();
                
                // Clean expired entries
                for (const [k, timestamp] of timestamps) {
                    if (now - timestamp > ttl) {
                        cache.delete(k);
                        timestamps.delete(k);
                    }
                }
                
                if (cache.has(key)) {
                    return cache.get(key);
                }
                
                const result = fn.apply(this, args);
                cache.set(key, result);
                timestamps.set(key, now);
                return result;
            };
        };
    }
    
    const ttlMemoize = createTTLMemoize(5000); // 5 second TTL
    console.log('TTL cache prevents stale data and memory growth');
}

// 3. Weak references for automatic cleanup
function weakRefSolution() {
    // Use WeakMap for objects that can be garbage collected
    const cache = new WeakMap();
    
    function memoizeForObjects(fn) {
        return function(obj, ...otherArgs) {
            if (typeof obj !== 'object' || obj === null) {
                return fn(obj, ...otherArgs);
            }
            
            let objCache = cache.get(obj);
            if (!objCache) {
                objCache = new Map();
                cache.set(obj, objCache);
            }
            
            const key = JSON.stringify(otherArgs);
            if (objCache.has(key)) {
                return objCache.get(key);
            }
            
            const result = fn(obj, ...otherArgs);
            objCache.set(key, result);
            return result;
        };
    }
    
    console.log('WeakMap allows automatic cleanup when objects are GC\'d');
}

// DECISION MATRIX:
function decisionMatrix() {
    const scenarios = [
        {
            scenario: 'Expensive pure function, repeated calls',
            recommendation: 'Use memoization ✅',
            reason: 'High benefit, low risk'
        },
        {
            scenario: 'Simple computation, high frequency',
            recommendation: 'Avoid memoization ❌',
            reason: 'Overhead > benefit'
        },
        {
            scenario: 'API calls with same parameters',
            recommendation: 'Use with TTL ✅',
            reason: 'Prevents redundant network requests'
        },
        {
            scenario: 'Recursive algorithms',
            recommendation: 'Use memoization ✅',
            reason: 'Dramatic performance improvement'
        },
        {
            scenario: 'Functions with side effects',
            recommendation: 'Avoid memoization ❌',
            reason: 'Side effects must execute every time'
        },
        {
            scenario: 'Long-running applications',
            recommendation: 'Use LRU/TTL ✅',
            reason: 'Prevent memory leaks'
        }
    ];
    
    console.table(scenarios);
}

// Utility function for basic memoization
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

function processUserData(userData) {
    return { ...userData, processed: true };
}

// Run demonstrations
demonstratePerformanceBenefit();
memoryOverhead();
keyGenerationOverhead();
memoryLeakRisk();
changingInputs();
simpleComputations();
lruSolution();
ttlSolution();
weakRefSolution();
decisionMatrix();

console.log(`
SUMMARY:
Benefits: Faster repeated computations, reduced redundant work
Costs: Memory usage, key generation overhead, complexity
Best for: Expensive pure functions with repeated inputs
Avoid for: Simple functions, changing inputs, side effects
Solutions: LRU caching, TTL, WeakMap, bounded cache sizes
`);
```
</details>

## Debouncing & throttling

**Beginner: Q1** - What is debouncing? When would you use it?

<details>
<summary>Answer</summary>

Debouncing delays function execution until after a specified time has passed since the last call:

```javascript
// DEBOUNCING = Delay execution until calls stop for X milliseconds

// Without debouncing (search on every keystroke)
function searchWithoutDebounce() {
    const input = document.getElementById('search');
    input.addEventListener('input', (e) => {
        console.log('Searching for:', e.target.value); // Fires on every keystroke
        // Expensive API call on every character typed
    });
}

// With debouncing (search only after user stops typing)
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

function searchWithDebounce() {
    const input = document.getElementById('search');
    
    const debouncedSearch = debounce((e) => {
        console.log('Searching for:', e.target.value); // Fires only after 300ms pause
        // API call only when user stops typing
    }, 300);
    
    input.addEventListener('input', debouncedSearch);
}

// Common use cases:
// 1. Search input
const searchInput = debounce((query) => {
    fetch(`/api/search?q=${query}`)
        .then(response => response.json())
        .then(data => displayResults(data));
}, 500);

// 2. Form validation
const validateForm = debounce((formData) => {
    console.log('Validating form...');
    // Expensive validation logic
}, 1000);

// 3. Window resize
const handleResize = debounce(() => {
    console.log('Window resized, recalculating layout...');
    // Expensive layout calculations
}, 250);

window.addEventListener('resize', handleResize);

// 4. Button click prevention
const submitButton = document.getElementById('submit');
const debouncedSubmit = debounce(() => {
    console.log('Form submitted');
    // Prevent multiple rapid clicks
}, 1000);

submitButton.addEventListener('click', debouncedSubmit);

function displayResults(data) {
    console.log('Search results:', data);
}
```
</details>

**Beginner: Q2** - What is throttling? When would you use it?

<details>
<summary>Answer</summary>

Throttling limits function execution to once per specified interval, regardless of how often it's called:

```javascript
// THROTTLING = Execute function at most once per X milliseconds

// Without throttling (scroll handler fires constantly)
function scrollWithoutThrottle() {
    window.addEventListener('scroll', () => {
        console.log('Scroll position:', window.scrollY); // Fires hundreds of times
        // Expensive calculations on every scroll pixel
    });
}

// With throttling (scroll handler limited to once per interval)
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

function scrollWithThrottle() {
    const throttledScroll = throttle(() => {
        console.log('Scroll position:', window.scrollY); // Fires max once per 100ms
        // Reasonable performance with useful updates
    }, 100);
    
    window.addEventListener('scroll', throttledScroll);
}

// Common use cases:
// 1. Scroll events
const updateScrollProgress = throttle(() => {
    const scrollPercent = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
    document.getElementById('progress').style.width = `${scrollPercent}%`;
}, 50);

window.addEventListener('scroll', updateScrollProgress);

// 2. Mouse move tracking
const trackMouseMovement = throttle((e) => {
    console.log(`Mouse at: ${e.clientX}, ${e.clientY}`);
    // Update UI element position
}, 16); // ~60fps

document.addEventListener('mousemove', trackMouseMovement);

// 3. API calls with rate limiting
const apiCall = throttle(() => {
    fetch('/api/data')
        .then(response => response.json())
        .then(data => console.log(data));
}, 1000); // Max 1 request per second

// 4. Button click rate limiting
const gameButton = throttle(() => {
    console.log('Game action performed');
    // Prevent spam clicking in games
}, 500);

// Advanced throttle with leading and trailing options
function advancedThrottle(func, limit, options = {}) {
    let timeout;
    let previous = 0;
    
    return function(...args) {
        const now = Date.now();
        
        if (!previous && options.leading === false) {
            previous = now;
        }
        
        const remaining = limit - (now - previous);
        
        if (remaining <= 0 || remaining > limit) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(this, args);
        } else if (!timeout && options.trailing !== false) {
            timeout = setTimeout(() => {
                previous = options.leading === false ? 0 : Date.now();
                timeout = null;
                func.apply(this, args);
            }, remaining);
        }
    };
}
```
</details>

**Beginner: Q3** - What is the difference between debouncing and throttling?

<details>
<summary>Answer</summary>

Debouncing waits for a pause in calls, while throttling executes at regular intervals:

```javascript
// VISUAL COMPARISON:
// User action: ||||||||||||||||||||||||||||||||||||||||||||||||
// Debouncing:                                                  X (only at end)
// Throttling:  X       X       X       X       X       X       X (regular intervals)

// DEBOUNCING: "Wait until quiet, then execute"
function createDebounceDemo() {
    let calls = 0;
    
    const debouncedFunction = debounce(() => {
        console.log(`Debounced execution #${++calls}`);
    }, 1000);
    
    // Simulate rapid calls
    for (let i = 0; i < 5; i++) {
        setTimeout(() => debouncedFunction(), i * 100);
    }
    // Only executes once, 1000ms after the last call
}

// THROTTLING: "Execute at most once per interval"
function createThrottleDemo() {
    let calls = 0;
    
    const throttledFunction = throttle(() => {
        console.log(`Throttled execution #${++calls}`);
    }, 1000);
    
    // Simulate rapid calls
    for (let i = 0; i < 10; i++) {
        setTimeout(() => throttledFunction(), i * 200);
    }
    // Executes immediately, then once per 1000ms
}

// PRACTICAL COMPARISON:

// Search input example
function searchComparison() {
    const searchInput = document.getElementById('search');
    
    // Debounced search - waits for user to stop typing
    const debouncedSearch = debounce((query) => {
        console.log('Debounced search:', query);
        // Good for: API calls, validation
        // Executes: Only after user stops typing for 300ms
    }, 300);
    
    // Throttled search - searches at regular intervals
    const throttledSearch = throttle((query) => {
        console.log('Throttled search:', query);
        // Good for: Live suggestions, progressive search
        // Executes: Max once per 300ms while typing
    }, 300);
    
    searchInput.addEventListener('input', (e) => {
        debouncedSearch(e.target.value);
        throttledSearch(e.target.value);
    });
}

// Scroll example
function scrollComparison() {
    // Debounced scroll - fires only when scrolling stops
    const debouncedScroll = debounce(() => {
        console.log('Scroll ended');
        // Good for: Final calculations, saving state
        // Use case: Auto-save when user stops scrolling
    }, 150);
    
    // Throttled scroll - fires regularly during scroll
    const throttledScroll = throttle(() => {
        console.log('Scrolling...');
        // Good for: Animations, progress bars, lazy loading
        // Use case: Update scroll progress indicator
    }, 100);
    
    window.addEventListener('scroll', debouncedScroll);
    window.addEventListener('scroll', throttledScroll);
}

// When to use which:
const useCase = {
    debouncing: {
        searchInput: 'Wait for user to finish typing',
        formValidation: 'Validate after user stops editing',
        resizeHandler: 'Recalculate layout after resize ends',
        buttonClick: 'Prevent accidental double-clicks'
    },
    
    throttling: {
        scrollEvents: 'Smooth animations during scroll',
        mouseMoveTracking: 'Limit tracking frequency',
        apiRateLimit: 'Respect API rate limits',
        gameControls: 'Limit action frequency in games'
    }
};

// Utility functions
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

createDebounceDemo();
createThrottleDemo();
```
</details>

**Intermediate: Q1** - Implement a debounce function.

<details>
<summary>Answer</summary>

Here's a comprehensive debounce implementation with advanced features:

```javascript
// BASIC DEBOUNCE IMPLEMENTATION:
function basicDebounce(func, delay) {
    let timeoutId;
    
    return function(...args) {
        // Clear previous timeout
        clearTimeout(timeoutId);
        
        // Set new timeout
        timeoutId = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// ADVANCED DEBOUNCE with options:
function advancedDebounce(func, delay, options = {}) {
    let timeoutId;
    let lastCallTime;
    
    const {
        leading = false,        // Execute immediately on first call
        trailing = true,        // Execute after delay
        maxWait = null         // Maximum time to wait before forced execution
    } = options;
    
    return function(...args) {
        const currentTime = Date.now();
        const timeSinceLastCall = currentTime - (lastCallTime || 0);
        
        // Clear existing timeout
        clearTimeout(timeoutId);
        
        // Leading edge execution
        if (leading && !lastCallTime) {
            func.apply(this, args);
            lastCallTime = currentTime;
            return;
        }
        
        // Set up trailing execution
        if (trailing) {
            timeoutId = setTimeout(() => {
                func.apply(this, args);
                lastCallTime = Date.now();
            }, delay);
        }
        
        // MaxWait forced execution
        if (maxWait && timeSinceLastCall >= maxWait) {
            func.apply(this, args);
            lastCallTime = currentTime;
            clearTimeout(timeoutId);
        } else {
            lastCallTime = currentTime;
        }
    };
}

// DEBOUNCE with CANCEL and FLUSH methods:
function fullDebounce(func, delay, options = {}) {
    let timeoutId;
    let lastArgs;
    let lastThis;
    
    function debounced(...args) {
        lastArgs = args;
        lastThis = this;
        
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
            func.apply(lastThis, lastArgs);
        }, delay);
    }
    
    // Cancel pending execution
    debounced.cancel = function() {
        clearTimeout(timeoutId);
        timeoutId = null;
    };
    
    // Execute immediately with last arguments
    debounced.flush = function() {
        if (timeoutId) {
            clearTimeout(timeoutId);
            func.apply(lastThis, lastArgs);
        }
    };
    
    // Check if execution is pending
    debounced.pending = function() {
        return timeoutId != null;
    };
    
    return debounced;
}

// USAGE EXAMPLES:

// 1. Search input with basic debounce
const searchInput = document.getElementById('search');
const debouncedSearch = basicDebounce((e) => {
    console.log('Searching for:', e.target.value);
    // API call here
}, 300);

searchInput?.addEventListener('input', debouncedSearch);

// 2. Form validation with advanced options
const validateForm = advancedDebounce(
    (formData) => {
        console.log('Validating form...', formData);
        // Validation logic
    },
    500,
    { leading: false, trailing: true, maxWait: 2000 }
);

// 3. Button with cancel/flush capabilities
const saveButton = document.getElementById('save');
const debouncedSave = fullDebounce(() => {
    console.log('Saving data...');
    // Save logic
}, 1000);

saveButton?.addEventListener('click', debouncedSave);

// Cancel save on page unload
window.addEventListener('beforeunload', () => {
    debouncedSave.cancel();
});

// Force save on form submit
document.getElementById('form')?.addEventListener('submit', () => {
    debouncedSave.flush();
});

// 4. Resize handler with immediate and delayed execution
const handleResize = advancedDebounce(
    () => {
        console.log('Window resized, recalculating...');
        // Layout recalculation
    },
    250,
    { leading: true, trailing: true } // Execute immediately AND after delay
);

window.addEventListener('resize', handleResize);

// 5. Auto-save with maxWait
const autoSave = advancedDebounce(
    (data) => {
        console.log('Auto-saving...', data);
        // Save to server
    },
    2000,  // Wait 2s after last change
    { maxWait: 10000 } // Force save every 10s
);

// Usage in text editor
document.getElementById('editor')?.addEventListener('input', (e) => {
    autoSave(e.target.value);
});

// TESTING UTILITIES:
function testDebounce() {
    const mockFunction = jest.fn();
    const debouncedMock = basicDebounce(mockFunction, 100);
    
    // Call multiple times rapidly
    debouncedMock(1);
    debouncedMock(2);
    debouncedMock(3);
    
    // Should only execute once with last argument
    setTimeout(() => {
        expect(mockFunction).toHaveBeenCalledTimes(1);
        expect(mockFunction).toHaveBeenCalledWith(3);
    }, 150);
}

// Performance monitoring
function createMonitoredDebounce(func, delay) {
    let callCount = 0;
    let executionCount = 0;
    
    const debounced = basicDebounce((...args) => {
        executionCount++;
        console.log(`Execution ${executionCount} (${callCount} calls total)`);
        func.apply(this, args);
    }, delay);
    
    return function(...args) {
        callCount++;
        return debounced.apply(this, args);
    };
}

// Real-world example: Search with loading state
function createSmartSearch() {
    const searchInput = document.getElementById('search');
    const results = document.getElementById('results');
    const loading = document.getElementById('loading');
    
    const debouncedSearch = fullDebounce(async (query) => {
        if (!query.trim()) {
            results.innerHTML = '';
            return;
        }
        
        loading.style.display = 'block';
        
        try {
            const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`);
            const data = await response.json();
            
            results.innerHTML = data.map(item => `<div>${item.title}</div>`).join('');
        } catch (error) {
            results.innerHTML = '<div>Search failed</div>';
        } finally {
            loading.style.display = 'none';
        }
    }, 300);
    
    searchInput?.addEventListener('input', (e) => {
        debouncedSearch(e.target.value);
    });
    
    // Clear search on escape
    searchInput?.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
            debouncedSearch.cancel();
            results.innerHTML = '';
            loading.style.display = 'none';
        }
    });
}
```
</details>

**Intermediate: Q2** - Implement a throttle function.

<details>
<summary>Answer</summary>

Here's a comprehensive throttle implementation with advanced features:

```javascript
// BASIC THROTTLE IMPLEMENTATION:
function basicThrottle(func, limit) {
    let inThrottle;
    
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// ADVANCED THROTTLE with leading/trailing options:
function advancedThrottle(func, limit, options = {}) {
    let timeout;
    let previous = 0;
    
    const {
        leading = true,     // Execute on leading edge
        trailing = true     // Execute on trailing edge
    } = options;
    
    return function(...args) {
        const now = Date.now();
        
        // If leading is false, set previous to now on first call
        if (!previous && leading === false) {
            previous = now;
        }
        
        const remaining = limit - (now - previous);
        
        if (remaining <= 0 || remaining > limit) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(this, args);
        } else if (!timeout && trailing !== false) {
            timeout = setTimeout(() => {
                previous = leading === false ? 0 : Date.now();
                timeout = null;
                func.apply(this, args);
            }, remaining);
        }
    };
}

// THROTTLE with CANCEL and FLUSH methods:
function fullThrottle(func, limit, options = {}) {
    let timeout;
    let previous = 0;
    let lastArgs;
    let lastThis;
    
    function throttled(...args) {
        lastArgs = args;
        lastThis = this;
        
        const now = Date.now();
        const remaining = limit - (now - previous);
        
        if (remaining <= 0) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(this, args);
        } else if (!timeout) {
            timeout = setTimeout(() => {
                previous = Date.now();
                timeout = null;
                func.apply(lastThis, lastArgs);
            }, remaining);
        }
    }
    
    // Cancel pending execution
    throttled.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
        previous = 0;
    };
    
    // Execute immediately
    throttled.flush = function() {
        if (timeout) {
            clearTimeout(timeout);
            func.apply(lastThis, lastArgs);
            timeout = null;
            previous = Date.now();
        }
    };
    
    // Check if execution is pending
    throttled.pending = function() {
        return timeout != null;
    };
    
    return throttled;
}

// USAGE EXAMPLES:

// 1. Scroll progress indicator
const updateScrollProgress = basicThrottle(() => {
    const scrollPercent = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
    const progressBar = document.getElementById('progress');
    if (progressBar) {
        progressBar.style.width = `${Math.min(100, Math.max(0, scrollPercent))}%`;
    }
}, 16); // ~60fps

window.addEventListener('scroll', updateScrollProgress);

// 2. Mouse tracking with advanced options
const trackMouse = advancedThrottle(
    (e) => {
        console.log(`Mouse: ${e.clientX}, ${e.clientY}`);
        // Update custom cursor or parallax effect
    },
    50,
    { leading: true, trailing: false } // Only on leading edge
);

document.addEventListener('mousemove', trackMouse);

// 3. API rate limiting
const apiThrottle = fullThrottle(
    async (data) => {
        console.log('Making API call...', data);
        try {
            const response = await fetch('/api/endpoint', {
                method: 'POST',
                body: JSON.stringify(data)
            });
            return response.json();
        } catch (error) {
            console.error('API call failed:', error);
        }
    },
    1000 // Max 1 call per second
);

// Usage with flush on page unload
window.addEventListener('beforeunload', () => {
    apiThrottle.flush(); // Send final data
});

// 4. Game controls throttling
const gameAction = advancedThrottle(
    (action) => {
        console.log('Game action:', action);
        // Process game action
    },
    100, // Max 10 actions per second
    { leading: true, trailing: false }
);

document.addEventListener('keydown', (e) => {
    if (e.code === 'Space') {
        gameAction('jump');
    }
});

// 5. Window resize with immediate and delayed execution
const handleResize = advancedThrottle(
    () => {
        console.log('Resize handler executed');
        // Expensive layout calculations
    },
    200,
    { leading: true, trailing: true } // Both edges
);

window.addEventListener('resize', handleResize);

// PERFORMANCE COMPARISON:
function performanceTest() {
    let regularCount = 0;
    let throttledCount = 0;
    
    const regularFunction = () => regularCount++;
    const throttledFunction = basicThrottle(() => throttledCount++, 100);
    
    // Simulate rapid calls
    const interval = setInterval(() => {
        regularFunction();
        throttledFunction();
    }, 10);
    
    // Check results after 1 second
    setTimeout(() => {
        clearInterval(interval);
        console.log(`Regular calls: ${regularCount}`); // ~100
        console.log(`Throttled calls: ${throttledCount}`); // ~10
    }, 1000);
}

// REAL-WORLD EXAMPLES:

// 1. Infinite scroll
function createInfiniteScroll() {
    const container = document.getElementById('content');
    let page = 1;
    let loading = false;
    
    const loadMore = fullThrottle(async () => {
        if (loading) return;
        
        const scrollPosition = window.scrollY + window.innerHeight;
        const documentHeight = document.documentElement.scrollHeight;
        
        if (scrollPosition >= documentHeight - 100) { // 100px threshold
            loading = true;
            console.log(`Loading page ${page}...`);
            
            try {
                const response = await fetch(`/api/content?page=${page}`);
                const data = await response.json();
                
                // Append new content
                data.items.forEach(item => {
                    const div = document.createElement('div');
                    div.textContent = item.title;
                    container?.appendChild(div);
                });
                
                page++;
            } catch (error) {
                console.error('Failed to load more content:', error);
            } finally {
                loading = false;
            }
        }
    }, 200);
    
    window.addEventListener('scroll', loadMore);
}

// 2. Live search suggestions
function createLiveSearch() {
    const searchInput = document.getElementById('search');
    const suggestions = document.getElementById('suggestions');
    
    const fetchSuggestions = fullThrottle(async (query) => {
        if (!query.trim()) {
            suggestions.innerHTML = '';
            return;
        }
        
        try {
            const response = await fetch(`/api/suggestions?q=${encodeURIComponent(query)}`);
            const data = await response.json();
            
            suggestions.innerHTML = data
                .map(item => `<div class="suggestion">${item}</div>`)
                .join('');
        } catch (error) {
            console.error('Failed to fetch suggestions:', error);
        }
    }, 300);
    
    searchInput?.addEventListener('input', (e) => {
        fetchSuggestions(e.target.value);
    });
}

// 3. Canvas animation throttling
function createCanvasAnimation() {
    const canvas = document.getElementById('canvas');
    const ctx = canvas?.getContext('2d');
    
    if (!ctx) return;
    
    let mouseX = 0;
    let mouseY = 0;
    
    const updateMouse = basicThrottle((e) => {
        const rect = canvas.getBoundingClientRect();
        mouseX = e.clientX - rect.left;
        mouseY = e.clientY - rect.top;
    }, 16); // 60fps
    
    const animate = () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        // Draw something that follows mouse
        ctx.beginPath();
        ctx.arc(mouseX, mouseY, 20, 0, 2 * Math.PI);
        ctx.fillStyle = 'blue';
        ctx.fill();
        
        requestAnimationFrame(animate);
    };
    
    canvas.addEventListener('mousemove', updateMouse);
    animate();
}

// Performance monitoring
function createMonitoredThrottle(func, limit) {
    let callCount = 0;
    let executionCount = 0;
    
    const throttled = basicThrottle((...args) => {
        executionCount++;
        console.log(`Execution ${executionCount}/${callCount} calls`);
        func.apply(this, args);
    }, limit);
    
    return function(...args) {
        callCount++;
        return throttled.apply(this, args);
    };
}

performanceTest();
```
</details>

## Polyfills

**Beginner: Q1** - What is a polyfill?

<details>
<summary>Answer</summary>

A polyfill is code that implements features on web browsers that don't natively support them:

```javascript
// POLYFILL = "Fill in" missing browser functionality

// Example: Array.includes() polyfill for older browsers
if (!Array.prototype.includes) {
    Array.prototype.includes = function(searchElement, fromIndex) {
        'use strict';
        
        // Convert to object
        const obj = Object(this);
        const len = parseInt(obj.length) || 0;
        
        if (len === 0) return false;
        
        // Calculate start index
        const startIndex = parseInt(fromIndex) || 0;
        let index = Math.max(startIndex >= 0 ? startIndex : len - Math.abs(startIndex), 0);
        
        // Search for element
        while (index < len) {
            if (obj[index] === searchElement) {
                return true;
            }
            index++;
        }
        
        return false;
    };
}

// Usage works the same across all browsers
const fruits = ['apple', 'banana', 'orange'];
console.log(fruits.includes('banana')); // true

// Common polyfills:
// 1. String.startsWith()
if (!String.prototype.startsWith) {
    String.prototype.startsWith = function(searchString, position) {
        position = position || 0;
        return this.substr(position, searchString.length) === searchString;
    };
}

// 2. Array.find()
if (!Array.prototype.find) {
    Array.prototype.find = function(callback, thisArg) {
        for (let i = 0; i < this.length; i++) {
            if (callback.call(thisArg, this[i], i, this)) {
                return this[i];
            }
        }
        return undefined;
    };
}

// 3. Object.assign()
if (!Object.assign) {
    Object.assign = function(target, ...sources) {
        if (target == null) {
            throw new TypeError('Cannot convert undefined or null to object');
        }
        
        const to = Object(target);
        
        sources.forEach(source => {
            if (source != null) {
                for (const key in source) {
                    if (source.hasOwnProperty(key)) {
                        to[key] = source[key];
                    }
                }
            }
        });
        
        return to;
    };
}

// Key characteristics:
console.log(`
Polyfill characteristics:
1. Provides missing functionality
2. Uses same API as native implementation
3. Only adds code if feature is missing
4. Enables consistent cross-browser experience
`);
```
</details>

**Beginner: Q2** - Why are polyfills needed?

<details>
<summary>Answer</summary>

Polyfills are needed to ensure consistent functionality across different browsers and versions:

```javascript
// BROWSER COMPATIBILITY ISSUES:

// 1. Different implementation timelines
// ES6 features weren't supported everywhere immediately
const modernFeatures = {
    'Array.includes()': 'Chrome 47+, Firefox 43+, Safari 9+',
    'String.startsWith()': 'Chrome 41+, Firefox 17+, Safari 9+',
    'Object.assign()': 'Chrome 45+, Firefox 34+, Safari 9+',
    'Promise': 'Chrome 32+, Firefox 29+, Safari 8+'
};

// 2. Legacy browser support
// Internet Explorer 11 lacks many modern features
const ie11Missing = [
    'Array.includes()',
    'String.startsWith()',
    'Object.assign()',
    'Promise',
    'fetch()',
    'Array.from()',
    'Set/Map'
];

// 3. Without polyfills - code breaks
function withoutPolyfill() {
    const data = ['apple', 'banana', 'orange'];
    
    try {
        // This would throw TypeError in IE11
        const hasApple = data.includes('apple');
        console.log('Has apple:', hasApple);
    } catch (error) {
        console.error('Method not supported:', error.message);
    }
}

// 4. With polyfills - code works everywhere
function withPolyfill() {
    // Polyfill ensures includes() exists
    if (!Array.prototype.includes) {
        Array.prototype.includes = function(searchElement) {
            return this.indexOf(searchElement) !== -1;
        };
    }
    
    const data = ['apple', 'banana', 'orange'];
    const hasApple = data.includes('apple'); // Works in all browsers
    console.log('Has apple:', hasApple);
}

// REAL-WORLD SCENARIOS:

// Scenario 1: Corporate environment with old browsers
function corporateSupport() {
    // Many companies still use IE11 or older Chrome versions
    const supportMatrix = {
        'IE 11': '6.3% global usage (2023)',
        'Chrome 80-90': 'Common in enterprise',
        'Safari 12-13': 'Older macOS versions'
    };
    
    console.log('Must support:', supportMatrix);
}

// Scenario 2: Global audience
function globalAudience() {
    // Different regions have different browser adoption rates
    const browserUsage = {
        'Developing countries': 'Often use older devices/browsers',
        'Mobile users': 'May have older mobile browsers',
        'Enterprise users': 'Restricted to older browser versions'
    };
    
    console.log('Browser diversity:', browserUsage);
}

// Scenario 3: Progressive enhancement
function progressiveEnhancement() {
    // Start with basic functionality, enhance with modern features
    
    // Basic functionality (works everywhere)
    function basicSearch(items, query) {
        for (let i = 0; i < items.length; i++) {
            if (items[i].toLowerCase().indexOf(query.toLowerCase()) !== -1) {
                return items[i];
            }
        }
        return null;
    }
    
    // Enhanced functionality with polyfill
    if (!Array.prototype.find) {
        Array.prototype.find = function(callback) {
            for (let i = 0; i < this.length; i++) {
                if (callback(this[i], i, this)) {
                    return this[i];
                }
            }
            return undefined;
        };
    }
    
    function enhancedSearch(items, query) {
        return items.find(item => 
            item.toLowerCase().includes(query.toLowerCase())
        );
    }
    
    // Use enhanced version with polyfill fallback
    const searchFunction = Array.prototype.find ? enhancedSearch : basicSearch;
}

// Benefits of polyfills:
const benefits = {
    consistency: 'Same API across all browsers',
    compatibility: 'Support older browsers without rewriting code',
    futureProof: 'Use modern features while maintaining backwards compatibility',
    productivity: 'Write modern JavaScript without browser-specific workarounds',
    userExperience: 'Consistent functionality for all users'
};

console.log('Polyfill benefits:', benefits);

withoutPolyfill();
withPolyfill();
corporateSupport();
globalAudience();
progressiveEnhancement();
```
</details>

**Beginner: Q3** - Give an example of when you might need a polyfill.

<details>
<summary>Answer</summary>

Common scenarios requiring polyfills include legacy browser support and missing modern JavaScript features:

```javascript
// SCENARIO 1: Company requires IE11 support
function ie11Support() {
    // Modern code using Array.includes()
    const validRoles = ['admin', 'user', 'guest'];
    const userRole = 'admin';
    
    // This breaks in IE11 - includes() not supported
    try {
        const isValidRole = validRoles.includes(userRole);
        console.log('Valid role:', isValidRole);
    } catch (error) {
        console.error('IE11 error:', error.message);
    }
    
    // Solution: Add polyfill
    if (!Array.prototype.includes) {
        Array.prototype.includes = function(searchElement) {
            return this.indexOf(searchElement) !== -1;
        };
    }
    
    // Now works in IE11
    const isValidRole = validRoles.includes(userRole);
    console.log('With polyfill:', isValidRole); // true
}

// SCENARIO 2: Promise support for older browsers
function promiseSupport() {
    // Modern async code
    function fetchUserData(userId) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve({ id: userId, name: 'John Doe' });
            }, 1000);
        });
    }
    
    // Promise polyfill for browsers without native support
    if (typeof Promise === 'undefined') {
        // Simple Promise polyfill
        window.Promise = function(executor) {
            const self = this;
            self.state = 'pending';
            self.value = undefined;
            self.onResolvedCallbacks = [];
            self.onRejectedCallbacks = [];
            
            function resolve(value) {
                if (self.state === 'pending') {
                    self.state = 'resolved';
                    self.value = value;
                    self.onResolvedCallbacks.forEach(callback => callback(value));
                }
            }
            
            function reject(reason) {
                if (self.state === 'pending') {
                    self.state = 'rejected';
                    self.value = reason;
                    self.onRejectedCallbacks.forEach(callback => callback(reason));
                }
            }
            
            try {
                executor(resolve, reject);
            } catch (error) {
                reject(error);
            }
        };
        
        Promise.prototype.then = function(onResolved, onRejected) {
            const self = this;
            return new Promise((resolve, reject) => {
                if (self.state === 'resolved') {
                    try {
                        const result = onResolved ? onResolved(self.value) : self.value;
                        resolve(result);
                    } catch (error) {
                        reject(error);
                    }
                } else if (self.state === 'rejected') {
                    try {
                        const result = onRejected ? onRejected(self.value) : self.value;
                        reject(result);
                    } catch (error) {
                        reject(error);
                    }
                } else {
                    self.onResolvedCallbacks.push((value) => {
                        try {
                            const result = onResolved ? onResolved(value) : value;
                            resolve(result);
                        } catch (error) {
                            reject(error);
                        }
                    });
                    
                    self.onRejectedCallbacks.push((reason) => {
                        try {
                            const result = onRejected ? onRejected(reason) : reason;
                            reject(result);
                        } catch (error) {
                            reject(error);
                        }
                    });
                }
            });
        };
    }
    
    // Now works in older browsers
    fetchUserData(123).then(user => {
        console.log('User data:', user);
    });
}

// SCENARIO 3: Fetch API for AJAX requests
function fetchPolyfill() {
    // Modern way to make HTTP requests
    if (!window.fetch) {
        window.fetch = function(url, options = {}) {
            return new Promise((resolve, reject) => {
                const xhr = new XMLHttpRequest();
                const method = options.method || 'GET';
                
                xhr.open(method, url);
                
                // Set headers
                if (options.headers) {
                    Object.keys(options.headers).forEach(key => {
                        xhr.setRequestHeader(key, options.headers[key]);
                    });
                }
                
                xhr.onload = function() {
                    const response = {
                        ok: xhr.status >= 200 && xhr.status < 300,
                        status: xhr.status,
                        statusText: xhr.statusText,
                        json: () => Promise.resolve(JSON.parse(xhr.responseText)),
                        text: () => Promise.resolve(xhr.responseText)
                    };
                    resolve(response);
                };
                
                xhr.onerror = () => reject(new Error('Network error'));
                
                xhr.send(options.body);
            });
        };
    }
    
    // Now fetch() works everywhere
    fetch('/api/data')
        .then(response => response.json())
        .then(data => console.log('Data:', data))
        .catch(error => console.error('Error:', error));
}

// SCENARIO 4: Object.assign() for object merging
function objectAssignPolyfill() {
    // Modern object merging
    const defaults = { timeout: 5000, retries: 3 };
    const userOptions = { timeout: 10000 };
    
    // Object.assign() not supported in IE
    if (!Object.assign) {
        Object.assign = function(target, ...sources) {
            if (target == null) {
                throw new TypeError('Cannot convert undefined or null to object');
            }
            
            const to = Object(target);
            
            sources.forEach(source => {
                if (source != null) {
                    for (const key in source) {
                        if (source.hasOwnProperty(key)) {
                            to[key] = source[key];
                        }
                    }
                }
            });
            
            return to;
        };
    }
    
    const config = Object.assign({}, defaults, userOptions);
    console.log('Config:', config); // { timeout: 10000, retries: 3 }
}

// SCENARIO 5: Real-world polyfill bundle
function realWorldExample() {
    // Common polyfills loaded at app startup
    const polyfills = [
        'Array.includes',
        'Array.find',
        'Array.from',
        'Object.assign',
        'String.startsWith',
        'String.endsWith',
        'Promise',
        'fetch'
    ];
    
    console.log('Loading polyfills for older browsers:', polyfills);
    
    // Typically loaded from CDN like polyfill.io
    // <script src="https://polyfill.io/v3/polyfill.min.js?features=Array.includes,Array.find,Promise,fetch"></script>
}

ie11Support();
promiseSupport();
fetchPolyfill();
objectAssignPolyfill();
realWorldExample();
```
</details>

**Intermediate: Q1** - Write a polyfill for `Array.prototype.includes()`.

<details>
<summary>Answer</summary>

Here's a complete, spec-compliant polyfill for `Array.prototype.includes()`:

```javascript
// SPEC-COMPLIANT Array.prototype.includes() POLYFILL
if (!Array.prototype.includes) {
    Array.prototype.includes = function(searchElement, fromIndex) {
        'use strict';
        
        // 1. Let O be ? ToObject(this value)
        if (this == null) {
            throw new TypeError('Array.prototype.includes called on null or undefined');
        }
        
        const obj = Object(this);
        
        // 2. Let len be ? ToLength(? Get(O, "length"))
        const len = parseInt(obj.length) || 0;
        
        // 3. If len is 0, return false
        if (len === 0) {
            return false;
        }
        
        // 4. Let n be ? ToInteger(fromIndex)
        const fromIdx = parseInt(fromIndex) || 0;
        
        // 5. If n ≥ 0, then let k be n
        // 6. Else n < 0, let k be len + n
        let k;
        if (fromIdx >= 0) {
            k = fromIdx;
        } else {
            k = len + fromIdx;
            // If k < 0, let k be 0
            if (k < 0) {
                k = 0;
            }
        }
        
        // 7. Repeat, while k < len
        while (k < len) {
            // a. Let elementK be the result of ? Get(O, ! ToString(k))
            const elementK = obj[k];
            
            // b. If SameValueZero(searchElement, elementK) is true, return true
            if (sameValueZero(searchElement, elementK)) {
                return true;
            }
            
            // c. Increase k by 1
            k++;
        }
        
        // 8. Return false
        return false;
        
        // SameValueZero comparison (handles NaN correctly)
        function sameValueZero(x, y) {
            // If x and y are exactly the same value
            if (x === y) {
                return true;
            }
            // Handle NaN case (NaN is equal to NaN in SameValueZero)
            if (typeof x === 'number' && typeof y === 'number' && 
                isNaN(x) && isNaN(y)) {
                return true;
            }
            return false;
        }
    };
}

// TESTING THE POLYFILL:

// Test 1: Basic functionality
function testBasic() {
    const arr = [1, 2, 3, 4, 5];
    
    console.log(arr.includes(3));     // true
    console.log(arr.includes(6));     // false
    console.log(arr.includes(1, 1));  // false (from index 1)
    console.log(arr.includes(3, 2));  // true (from index 2)
}

// Test 2: Edge cases
function testEdgeCases() {
    const arr = [1, 2, NaN, 4, 5];
    
    // NaN handling (special case in SameValueZero)
    console.log(arr.includes(NaN));        // true
    console.log(arr.indexOf(NaN));         // -1 (indexOf can't find NaN)
    
    // Negative fromIndex
    console.log(arr.includes(5, -1));      // true (from last element)
    console.log(arr.includes(1, -10));     // true (negative too large, starts from 0)
    
    // Zero handling
    const zeroArr = [+0, -0];
    console.log(zeroArr.includes(+0));     // true
    console.log(zeroArr.includes(-0));     // true (+0 === -0)
    
    // Empty array
    console.log([].includes(1));           // false
}

// Test 3: Type coercion and edge values
function testTypeCoercion() {
    const mixedArr = [1, '2', true, null, undefined];
    
    console.log(mixedArr.includes(1));     // true
    console.log(mixedArr.includes('1'));   // false (no type coercion)
    console.log(mixedArr.includes('2'));   // true
    console.log(mixedArr.includes(2));     // false
    console.log(mixedArr.includes(null));  // true
    console.log(mixedArr.includes(undefined)); // true
}

// Test 4: Array-like objects
function testArrayLike() {
    // String (array-like)
    const str = 'hello';
    console.log(Array.prototype.includes.call(str, 'e')); // true
    console.log(Array.prototype.includes.call(str, 'x')); // false
    
    // NodeList (array-like)
    const arrayLike = {
        0: 'a',
        1: 'b',
        2: 'c',
        length: 3
    };
    console.log(Array.prototype.includes.call(arrayLike, 'b')); // true
    console.log(Array.prototype.includes.call(arrayLike, 'd')); // false
}

// Test 5: Error cases
function testErrors() {
    try {
        Array.prototype.includes.call(null, 1);
    } catch (error) {
        console.log('Null error:', error.message);
    }
    
    try {
        Array.prototype.includes.call(undefined, 1);
    } catch (error) {
        console.log('Undefined error:', error.message);
    }
}

// SIMPLIFIED VERSION for basic needs:
function simpleIncludesPolyfill() {
    if (!Array.prototype.includes) {
        Array.prototype.includes = function(searchElement, fromIndex) {
            return this.indexOf(searchElement, fromIndex) !== -1;
        };
    }
    
    // Note: This simple version doesn't handle NaN correctly
    // indexOf returns -1 for NaN, but includes should return true for NaN
}

// COMPARISON with native behavior:
function compareWithNative() {
    const testCases = [
        { array: [1, 2, 3], search: 2, expected: true },
        { array: [1, 2, 3], search: 4, expected: false },
        { array: [1, NaN, 3], search: NaN, expected: true },
        { array: [1, 2, 3], search: 2, fromIndex: 2, expected: false },
        { array: [1, 2, 3], search: 3, fromIndex: -1, expected: true }
    ];
    
    testCases.forEach((test, index) => {
        const result = test.array.includes(test.search, test.fromIndex);
        const passed = result === test.expected;
        console.log(`Test ${index + 1}: ${passed ? 'PASS' : 'FAIL'} - Expected ${test.expected}, got ${result}`);
    });
}

// Run tests
testBasic();
testEdgeCases();
testTypeCoercion();
testArrayLike();
testErrors();
compareWithNative();
```
</details>

**Intermediate: Q2** - How do you detect if a polyfill is needed before adding it?

<details>
<summary>Answer</summary>

Feature detection is the standard way to conditionally load polyfills only when needed:

```javascript
// BASIC FEATURE DETECTION:
function detectFeatures() {
    // Check if method exists
    const needsIncludesPolyfill = !Array.prototype.includes;
    const needsStartsWithPolyfill = !String.prototype.startsWith;
    const needsAssignPolyfill = !Object.assign;
    
    console.log('Polyfills needed:', {
        includes: needsIncludesPolyfill,
        startsWith: needsStartsWithPolyfill,
        assign: needsAssignPolyfill
    });
}

// COMPREHENSIVE FEATURE DETECTION:
function comprehensiveDetection() {
    const features = {
        // Array methods
        'Array.includes': !!Array.prototype.includes,
        'Array.find': !!Array.prototype.find,
        'Array.findIndex': !!Array.prototype.findIndex,
        'Array.from': !!Array.from,
        
        // String methods
        'String.startsWith': !!String.prototype.startsWith,
        'String.endsWith': !!String.prototype.endsWith,
        'String.includes': !!String.prototype.includes,
        
        // Object methods
        'Object.assign': !!Object.assign,
        'Object.keys': !!Object.keys,
        'Object.values': !!Object.values,
        
        // Modern APIs
        'Promise': typeof Promise !== 'undefined',
        'fetch': typeof fetch !== 'undefined',
        'Map': typeof Map !== 'undefined',
        'Set': typeof Set !== 'undefined',
        
        // ES6+ features
        'arrow functions': (() => {
            try {
                eval('() => {}');
                return true;
            } catch (e) {
                return false;
            }
        })(),
        
        'const/let': (() => {
            try {
                eval('const x = 1; let y = 2;');
                return true;
            } catch (e) {
                return false;
            }
        })()
    };
    
    return features;
}

// CONDITIONAL POLYFILL LOADING:
function conditionalPolyfills() {
    // Method 1: Inline polyfills
    if (!Array.prototype.includes) {
        Array.prototype.includes = function(searchElement, fromIndex) {
            return this.indexOf(searchElement, fromIndex) !== -1;
        };
        console.log('Loaded Array.includes polyfill');
    }
    
    if (!String.prototype.startsWith) {
        String.prototype.startsWith = function(searchString, position) {
            position = position || 0;
            return this.substr(position, searchString.length) === searchString;
        };
        console.log('Loaded String.startsWith polyfill');
    }
    
    // Method 2: Dynamic script loading
    function loadPolyfillScript(feature, scriptUrl) {
        if (!window[feature]) {
            const script = document.createElement('script');
            script.src = scriptUrl;
            script.async = true;
            document.head.appendChild(script);
            console.log(`Loading ${feature} polyfill from ${scriptUrl}`);
        }
    }
    
    // Example usage
    loadPolyfillScript('Promise', 'https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js');
    loadPolyfillScript('fetch', 'https://cdn.jsdelivr.net/npm/whatwg-fetch@3/dist/fetch.umd.js');
}

// POLYFILL SERVICE INTEGRATION:
function polyfillService() {
    // Using polyfill.io service
    function loadPolyfillIO(features) {
        const baseUrl = 'https://polyfill.io/v3/polyfill.min.js';
        const featuresParam = features.join(',');
        const scriptUrl = `${baseUrl}?features=${featuresParam}`;
        
        const script = document.createElement('script');
        script.src = scriptUrl;
        script.async = true;
        document.head.appendChild(script);
        
        console.log(`Loading polyfills: ${featuresParam}`);
    }
    
    // Detect needed polyfills
    const neededPolyfills = [];
    
    if (!Array.prototype.includes) neededPolyfills.push('Array.prototype.includes');
    if (!String.prototype.startsWith) neededPolyfills.push('String.prototype.startsWith');
    if (!Object.assign) neededPolyfills.push('Object.assign');
    if (typeof Promise === 'undefined') neededPolyfills.push('Promise');
    if (typeof fetch === 'undefined') neededPolyfills.push('fetch');
    
    if (neededPolyfills.length > 0) {
        loadPolyfillIO(neededPolyfills);
    }
}

// ADVANCED FEATURE DETECTION:
function advancedDetection() {
    // Test actual functionality, not just existence
    function testArrayIncludes() {
        if (!Array.prototype.includes) return false;
        
        try {
            // Test NaN handling (critical for spec compliance)
            const result = [NaN].includes(NaN);
            return result === true;
        } catch (e) {
            return false;
        }
    }
    
    function testPromise() {
        if (typeof Promise === 'undefined') return false;
        
        try {
            // Test basic Promise functionality
            const p = new Promise((resolve) => resolve(42));
            return typeof p.then === 'function';
        } catch (e) {
            return false;
        }
    }
    
    function testFetch() {
        if (typeof fetch === 'undefined') return false;
        
        try {
            // Test if fetch returns a Promise
            const result = fetch('data:text/plain,test');
            return result && typeof result.then === 'function';
        } catch (e) {
            return false;
        }
    }
    
    return {
        'Array.includes (working)': testArrayIncludes(),
        'Promise (working)': testPromise(),
        'fetch (working)': testFetch()
    };
}

// POLYFILL MANAGER CLASS:
class PolyfillManager {
    constructor() {
        this.loaded = new Set();
        this.loading = new Map();
    }
    
    detect(feature) {
        const detectors = {
            'Array.includes': () => !!Array.prototype.includes,
            'Array.find': () => !!Array.prototype.find,
            'String.startsWith': () => !!String.prototype.startsWith,
            'Object.assign': () => !!Object.assign,
            'Promise': () => typeof Promise !== 'undefined',
            'fetch': () => typeof fetch !== 'undefined'
        };
        
        return detectors[feature] ? detectors[feature]() : false;
    }
    
    async loadPolyfill(feature, polyfillFn) {
        if (this.detect(feature)) {
            console.log(`${feature} already supported`);
            return Promise.resolve();
        }
        
        if (this.loaded.has(feature)) {
            console.log(`${feature} polyfill already loaded`);
            return Promise.resolve();
        }
        
        if (this.loading.has(feature)) {
            console.log(`${feature} polyfill already loading`);
            return this.loading.get(feature);
        }
        
        console.log(`Loading ${feature} polyfill`);
        
        const promise = new Promise((resolve) => {
            try {
                polyfillFn();
                this.loaded.add(feature);
                console.log(`${feature} polyfill loaded successfully`);
                resolve();
            } catch (error) {
                console.error(`Failed to load ${feature} polyfill:`, error);
                resolve(); // Don't reject, continue with degraded functionality
            }
        });
        
        this.loading.set(feature, promise);
        
        promise.then(() => {
            this.loading.delete(feature);
        });
        
        return promise;
    }
    
    async loadAll(polyfills) {
        const promises = Object.entries(polyfills).map(([feature, polyfillFn]) => {
            return this.loadPolyfill(feature, polyfillFn);
        });
        
        return Promise.all(promises);
    }
}

// USAGE EXAMPLE:
async function usePolyfillManager() {
    const manager = new PolyfillManager();
    
    const polyfills = {
        'Array.includes': () => {
            if (!Array.prototype.includes) {
                Array.prototype.includes = function(searchElement, fromIndex) {
                    return this.indexOf(searchElement, fromIndex) !== -1;
                };
            }
        },
        
        'String.startsWith': () => {
            if (!String.prototype.startsWith) {
                String.prototype.startsWith = function(searchString, position) {
                    position = position || 0;
                    return this.substr(position, searchString.length) === searchString;
                };
            }
        }
    };
    
    await manager.loadAll(polyfills);
    
    // Now use the features
    console.log([1, 2, 3].includes(2)); // Works everywhere
    console.log('hello'.startsWith('he')); // Works everywhere
}

// Run detection
detectFeatures();
console.log('Comprehensive detection:', comprehensiveDetection());
console.log('Advanced detection:', advancedDetection());
conditionalPolyfills();
polyfillService();
usePolyfillManager();
```
</details>

## Event delegation

**Beginner: Q1** - What is event delegation?

<details>
<summary>Answer</summary>

Event delegation uses event bubbling to handle events on child elements through a parent element listener:

```javascript
// EVENT DELEGATION = Single listener on parent handles child events

// Without delegation (inefficient)
function withoutDelegation() {
    const buttons = document.querySelectorAll('.button');
    
    // Add listener to each button
    buttons.forEach(button => {
        button.addEventListener('click', (e) => {
            console.log('Button clicked:', e.target.textContent);
        });
    });
    
    // Problem: Many listeners, memory usage, no handling of dynamic elements
}

// With delegation (efficient)
function withDelegation() {
    const container = document.getElementById('button-container');
    
    // Single listener on parent
    container.addEventListener('click', (e) => {
        // Check if clicked element is a button
        if (e.target.classList.contains('button')) {
            console.log('Button clicked:', e.target.textContent);
        }
    });
    
    // Benefits: One listener, handles dynamic elements, better performance
}

// HTML Structure:
/*
<div id="button-container">
    <button class="button">Button 1</button>
    <button class="button">Button 2</button>
    <button class="button">Button 3</button>
    <!-- New buttons added dynamically work automatically -->
</div>
*/

// Real-world example: List management
function listExample() {
    const todoList = document.getElementById('todo-list');
    
    // Delegate click events for todo items
    todoList.addEventListener('click', (e) => {
        const todoItem = e.target.closest('.todo-item');
        if (!todoItem) return;
        
        if (e.target.classList.contains('delete-btn')) {
            // Handle delete
            todoItem.remove();
            console.log('Todo deleted');
        } else if (e.target.classList.contains('edit-btn')) {
            // Handle edit
            const text = todoItem.querySelector('.todo-text');
            text.contentEditable = true;
            text.focus();
            console.log('Todo editing enabled');
        } else if (e.target.classList.contains('complete-btn')) {
            // Handle complete
            todoItem.classList.toggle('completed');
            console.log('Todo completion toggled');
        }
    });
}

// Dynamic content example
function dynamicContent() {
    const container = document.getElementById('dynamic-container');
    
    // Delegation handles future elements
    container.addEventListener('click', (e) => {
        if (e.target.matches('.dynamic-button')) {
            console.log('Dynamic button clicked:', e.target.dataset.id);
        }
    });
    
    // Add elements dynamically
    function addButton(id) {
        const button = document.createElement('button');
        button.className = 'dynamic-button';
        button.textContent = `Button ${id}`;
        button.dataset.id = id;
        container.appendChild(button);
    }
    
    // These buttons will work with delegation
    addButton(1);
    addButton(2);
    addButton(3);
}

withDelegation();
listExample();
dynamicContent();
```
</details>

**Beginner: Q2** - Why is event delegation useful?

<details>
<summary>Answer</summary>

Event delegation improves performance, memory usage, and handles dynamic content automatically:

```javascript
// BENEFITS OF EVENT DELEGATION:

// 1. PERFORMANCE - Fewer event listeners
function performanceBenefit() {
    // Without delegation: 1000 listeners
    function inefficient() {
        const items = document.querySelectorAll('.item'); // 1000 items
        
        items.forEach(item => {
            item.addEventListener('click', handleClick); // 1000 listeners
        });
        
        function handleClick(e) {
            console.log('Item clicked');
        }
    }
    
    // With delegation: 1 listener
    function efficient() {
        const container = document.querySelector('.container');
        
        // Single listener handles all items
        container.addEventListener('click', (e) => {
            if (e.target.classList.contains('item')) {
                console.log('Item clicked');
            }
        }); // Only 1 listener
    }
    
    console.log('Delegation reduces listeners from 1000 to 1');
}

// 2. MEMORY EFFICIENCY
function memoryBenefit() {
    // Each listener consumes memory
    const memoryUsage = {
        withoutDelegation: '1000 listeners × memory per listener',
        withDelegation: '1 listener × memory per listener',
        savings: 'Significant memory reduction'
    };
    
    console.log('Memory comparison:', memoryUsage);
}

// 3. DYNAMIC CONTENT SUPPORT
function dynamicContentBenefit() {
    const list = document.getElementById('dynamic-list');
    
    // Delegation automatically handles new elements
    list.addEventListener('click', (e) => {
        if (e.target.classList.contains('list-item')) {
            console.log('List item clicked:', e.target.textContent);
        }
    });
    
    // Add items dynamically - they work automatically
    function addItem(text) {
        const item = document.createElement('div');
        item.className = 'list-item';
        item.textContent = text;
        list.appendChild(item);
    }
    
    // These new items work with existing delegation
    addItem('Dynamic Item 1');
    addItem('Dynamic Item 2');
    
    console.log('New items work without additional listeners');
}

// 4. EASIER EVENT MANAGEMENT
function managementBenefit() {
    // Without delegation: Complex listener management
    function complexManagement() {
        const buttons = [];
        
        function addButton(id) {
            const button = document.createElement('button');
            button.id = id;
            button.textContent = `Button ${id}`;
            
            // Must add listener to each new button
            const handler = () => console.log(`Button ${id} clicked`);
            button.addEventListener('click', handler);
            
            // Must track for cleanup
            buttons.push({ element: button, handler });
            
            document.body.appendChild(button);
        }
        
        function removeButton(id) {
            const buttonData = buttons.find(b => b.element.id === id);
            if (buttonData) {
                // Must remove listener manually
                buttonData.element.removeEventListener('click', buttonData.handler);
                buttonData.element.remove();
                buttons.splice(buttons.indexOf(buttonData), 1);
            }
        }
        
        addButton('btn1');
        addButton('btn2');
        removeButton('btn1'); // Complex cleanup
    }
    
    // With delegation: Simple management
    function simpleManagement() {
        document.body.addEventListener('click', (e) => {
            if (e.target.matches('button[id^="btn"]')) {
                console.log(`Button ${e.target.id} clicked`);
            }
        });
        
        function addButton(id) {
            const button = document.createElement('button');
            button.id = id;
            button.textContent = `Button ${id}`;
            document.body.appendChild(button);
            // No listener management needed
        }
        
        function removeButton(id) {
            document.getElementById(id)?.remove();
            // No listener cleanup needed
        }
        
        addButton('btn1');
        addButton('btn2');
        removeButton('btn1'); // Simple cleanup
    }
    
    simpleManagement();
}

// 5. BETTER PERFORMANCE FOR LARGE LISTS
function largeListPerformance() {
    // Table with 10,000 rows
    function createLargeTable() {
        const table = document.getElementById('large-table');
        
        // Single delegated listener
        table.addEventListener('click', (e) => {
            const row = e.target.closest('tr');
            if (!row) return;
            
            if (e.target.classList.contains('edit-btn')) {
                console.log('Edit row:', row.dataset.id);
            } else if (e.target.classList.contains('delete-btn')) {
                console.log('Delete row:', row.dataset.id);
                row.remove();
            }
        });
        
        // Create 10,000 rows without individual listeners
        for (let i = 0; i < 10000; i++) {
            const row = document.createElement('tr');
            row.dataset.id = i;
            row.innerHTML = `
                <td>Item ${i}</td>
                <td>
                    <button class="edit-btn">Edit</button>
                    <button class="delete-btn">Delete</button>
                </td>
            `;
            table.appendChild(row);
        }
        
        console.log('10,000 rows with 1 listener vs 20,000 individual listeners');
    }
    
    createLargeTable();
}

// 6. EVENT DELEGATION PATTERNS
function commonPatterns() {
    // Pattern 1: Form validation
    const form = document.getElementById('dynamic-form');
    form.addEventListener('input', (e) => {
        if (e.target.matches('input[required]')) {
            validateField(e.target);
        }
    });
    
    // Pattern 2: Modal dialogs
    document.addEventListener('click', (e) => {
        if (e.target.matches('[data-modal-trigger]')) {
            openModal(e.target.dataset.modalTrigger);
        } else if (e.target.matches('.modal-close')) {
            closeModal(e.target.closest('.modal'));
        }
    });
    
    // Pattern 3: Navigation menus
    const nav = document.querySelector('.navigation');
    nav.addEventListener('click', (e) => {
        if (e.target.matches('.nav-link')) {
            handleNavigation(e.target);
        }
    });
    
    function validateField(field) {
        console.log('Validating field:', field.name);
    }
    
    function openModal(modalId) {
        console.log('Opening modal:', modalId);
    }
    
    function closeModal(modal) {
        console.log('Closing modal');
    }
    
    function handleNavigation(link) {
        console.log('Navigating to:', link.href);
    }
}

performanceBenefit();
memoryBenefit();
dynamicContentBenefit();
managementBenefit();
largeListPerformance();
commonPatterns();
```
</details>

**Beginner: Q3** - How do you implement event delegation?

<details>
<summary>Answer</summary>

Implement event delegation by adding a listener to a parent element and checking the event target:

```javascript
// BASIC EVENT DELEGATION IMPLEMENTATION:

// Step 1: Add listener to parent element
function basicDelegation() {
    const container = document.getElementById('container');
    
    container.addEventListener('click', (e) => {
        // Step 2: Check if target matches our criteria
        if (e.target.classList.contains('button')) {
            console.log('Button clicked:', e.target.textContent);
        }
    });
}

// COMMON DELEGATION PATTERNS:

// Pattern 1: Class-based delegation
function classBased() {
    const list = document.getElementById('todo-list');
    
    list.addEventListener('click', (e) => {
        // Check by class
        if (e.target.classList.contains('delete-btn')) {
            const item = e.target.closest('.todo-item');
            item.remove();
        } else if (e.target.classList.contains('edit-btn')) {
            const item = e.target.closest('.todo-item');
            enableEdit(item);
        }
    });
    
    function enableEdit(item) {
        const text = item.querySelector('.todo-text');
        text.contentEditable = true;
        text.focus();
    }
}

// Pattern 2: Data attribute delegation
function dataAttributeBased() {
    const container = document.getElementById('container');
    
    container.addEventListener('click', (e) => {
        // Check by data attribute
        const action = e.target.dataset.action;
        
        if (action === 'delete') {
            deleteItem(e.target.dataset.id);
        } else if (action === 'edit') {
            editItem(e.target.dataset.id);
        } else if (action === 'view') {
            viewItem(e.target.dataset.id);
        }
    });
    
    function deleteItem(id) {
        console.log('Deleting item:', id);
        document.querySelector(`[data-id="${id}"]`).remove();
    }
    
    function editItem(id) {
        console.log('Editing item:', id);
    }
    
    function viewItem(id) {
        console.log('Viewing item:', id);
    }
}

// Pattern 3: Tag-based delegation
function tagBased() {
    const form = document.getElementById('form');
    
    form.addEventListener('input', (e) => {
        // Check by tag name
        if (e.target.tagName === 'INPUT') {
            validateInput(e.target);
        } else if (e.target.tagName === 'TEXTAREA') {
            countCharacters(e.target);
        } else if (e.target.tagName === 'SELECT') {
            handleSelectChange(e.target);
        }
    });
    
    function validateInput(input) {
        console.log('Validating input:', input.name);
    }
    
    function countCharacters(textarea) {
        console.log('Character count:', textarea.value.length);
    }
    
    function handleSelectChange(select) {
        console.log('Selection changed:', select.value);
    }
}

// ADVANCED DELEGATION TECHNIQUES:

// Using element.matches() for complex selectors
function advancedMatching() {
    const container = document.getElementById('container');
    
    container.addEventListener('click', (e) => {
        // Complex CSS selector matching
        if (e.target.matches('button.primary[data-action]')) {
            handlePrimaryAction(e.target);
        } else if (e.target.matches('.card .header .close-btn')) {
            closeCard(e.target.closest('.card'));
        } else if (e.target.matches('a[href^="mailto:"]')) {
            handleEmailLink(e.target);
        }
    });
    
    function handlePrimaryAction(button) {
        console.log('Primary action:', button.dataset.action);
    }
    
    function closeCard(card) {
        card.remove();
    }
    
    function handleEmailLink(link) {
        console.log('Email link clicked:', link.href);
    }
}

// Using element.closest() for nested elements
function closestDelegation() {
    const table = document.getElementById('data-table');
    
    table.addEventListener('click', (e) => {
        // Find closest parent that matches
        const row = e.target.closest('tr[data-id]');
        if (!row) return; // Not in a data row
        
        const rowId = row.dataset.id;
        
        if (e.target.matches('.edit-btn')) {
            editRow(rowId);
        } else if (e.target.matches('.delete-btn')) {
            deleteRow(rowId);
        } else if (e.target.matches('.view-btn')) {
            viewRow(rowId);
        }
    });
    
    function editRow(id) {
        console.log('Edit row:', id);
    }
    
    function deleteRow(id) {
        console.log('Delete row:', id);
        document.querySelector(`tr[data-id="${id}"]`).remove();
    }
    
    function viewRow(id) {
        console.log('View row:', id);
    }
}

// DELEGATION UTILITY FUNCTION:
function createDelegator(container, eventType = 'click') {
    const handlers = new Map();
    
    container.addEventListener(eventType, (e) => {
        for (const [selector, handler] of handlers) {
            if (e.target.matches(selector)) {
                handler(e);
                break; // Stop after first match
            }
        }
    });
    
    return {
        on(selector, handler) {
            handlers.set(selector, handler);
        },
        
        off(selector) {
            handlers.delete(selector);
        },
        
        clear() {
            handlers.clear();
        }
    };
}

// Usage of delegation utility
function useUtility() {
    const container = document.getElementById('app');
    const delegator = createDelegator(container);
    
    // Add handlers
    delegator.on('.btn-primary', (e) => {
        console.log('Primary button clicked');
    });
    
    delegator.on('.btn-secondary', (e) => {
        console.log('Secondary button clicked');
    });
    
    delegator.on('[data-toggle="modal"]', (e) => {
        const modalId = e.target.dataset.target;
        console.log('Opening modal:', modalId);
    });
    
    // Remove handler
    delegator.off('.btn-secondary');
}

// REAL-WORLD EXAMPLES:

// Example 1: Shopping cart
function shoppingCart() {
    const cart = document.getElementById('shopping-cart');
    
    cart.addEventListener('click', (e) => {
        const item = e.target.closest('.cart-item');
        if (!item) return;
        
        const itemId = item.dataset.itemId;
        
        if (e.target.matches('.quantity-increase')) {
            increaseQuantity(itemId);
        } else if (e.target.matches('.quantity-decrease')) {
            decreaseQuantity(itemId);
        } else if (e.target.matches('.remove-item')) {
            removeItem(itemId);
        }
    });
    
    function increaseQuantity(itemId) {
        const quantityEl = document.querySelector(`[data-item-id="${itemId}"] .quantity`);
        quantityEl.textContent = parseInt(quantityEl.textContent) + 1;
    }
    
    function decreaseQuantity(itemId) {
        const quantityEl = document.querySelector(`[data-item-id="${itemId}"] .quantity`);
        const quantity = parseInt(quantityEl.textContent);
        if (quantity > 1) {
            quantityEl.textContent = quantity - 1;
        }
    }
    
    function removeItem(itemId) {
        document.querySelector(`[data-item-id="${itemId}"]`).remove();
    }
}

// Example 2: Image gallery
function imageGallery() {
    const gallery = document.getElementById('image-gallery');
    
    gallery.addEventListener('click', (e) => {
        if (e.target.matches('.thumbnail')) {
            showLightbox(e.target.src, e.target.alt);
        } else if (e.target.matches('.favorite-btn')) {
            toggleFavorite(e.target.closest('.image-item'));
        }
    });
    
    function showLightbox(src, alt) {
        console.log('Showing lightbox for:', alt);
        // Lightbox implementation
    }
    
    function toggleFavorite(imageItem) {
        imageItem.classList.toggle('favorited');
        console.log('Favorite toggled');
    }
}

basicDelegation();
classBased();
dataAttributeBased();
tagBased();
advancedMatching();
closestDelegation();
useUtility();
shoppingCart();
imageGallery();
```
</details>

**Intermediate: Q1** - What are the advantages and disadvantages of event delegation?

<details>
<summary>Answer</summary>

Event delegation has significant advantages but also some limitations to consider:

```javascript
// ADVANTAGES OF EVENT DELEGATION:

// 1. PERFORMANCE BENEFITS
function performanceAdvantages() {
    // Advantage: Fewer event listeners
    console.log('Traditional approach: 1000 elements = 1000 listeners');
    console.log('Delegation approach: 1000 elements = 1 listener');
    
    // Memory usage comparison
    const memoryComparison = {
        traditional: '1000 × 8KB = 8MB (example)',
        delegation: '1 × 8KB = 8KB',
        savings: '99.9% memory reduction'
    };
    
    console.log('Memory usage:', memoryComparison);
    
    // Event listener registration time
    console.time('Traditional registration');
    // Simulate 1000 individual listeners
    for (let i = 0; i < 1000; i++) {
        // document.getElementById(`item-${i}`).addEventListener('click', handler);
    }
    console.timeEnd('Traditional registration');
    
    console.time('Delegation registration');
    // Single delegation listener
    // document.getElementById('container').addEventListener('click', delegatedHandler);
    console.timeEnd('Delegation registration');
}

// 2. DYNAMIC CONTENT SUPPORT
function dynamicContentAdvantage() {
    const container = document.getElementById('dynamic-container');
    
    // Set up delegation once
    container.addEventListener('click', (e) => {
        if (e.target.matches('.dynamic-button')) {
            console.log('Dynamic button clicked:', e.target.textContent);
        }
    });
    
    // Adding new elements works automatically
    function addNewButton(id) {
        const button = document.createElement('button');
        button.className = 'dynamic-button';
        button.textContent = `Button ${id}`;
        container.appendChild(button);
        // No need to add event listener - delegation handles it
    }
    
    // Advantage: New elements work immediately
    setInterval(() => {
        const id = Date.now();
        addNewButton(id);
        console.log(`Added button ${id} - works with existing delegation`);
    }, 2000);
}

// 3. SIMPLIFIED EVENT MANAGEMENT
function managementAdvantage() {
    // Traditional approach: Complex cleanup
    class TraditionalList {
        constructor() {
            this.items = [];
            this.listeners = new Map();
        }
        
        addItem(id, text) {
            const item = document.createElement('div');
            item.id = id;
            item.textContent = text;
            
            const handler = () => this.handleClick(id);
            item.addEventListener('click', handler);
            
            // Must track for cleanup
            this.listeners.set(id, handler);
            this.items.push(item);
        }
        
        removeItem(id) {
            const item = document.getElementById(id);
            const handler = this.listeners.get(id);
            
            // Must manually remove listener
            item.removeEventListener('click', handler);
            item.remove();
            
            this.listeners.delete(id);
            this.items = this.items.filter(item => item.id !== id);
        }
        
        handleClick(id) {
            console.log('Traditional click:', id);
        }
    }
    
    // Delegation approach: Simple cleanup
    class DelegatedList {
        constructor(container) {
            this.container = container;
            this.items = [];
            
            // Single listener handles everything
            container.addEventListener('click', (e) => {
                if (e.target.matches('.list-item')) {
                    this.handleClick(e.target.dataset.id);
                }
            });
        }
        
        addItem(id, text) {
            const item = document.createElement('div');
            item.className = 'list-item';
            item.dataset.id = id;
            item.textContent = text;
            
            this.container.appendChild(item);
            this.items.push(item);
            // No listener management needed
        }
        
        removeItem(id) {
            const item = this.container.querySelector(`[data-id="${id}"]`);
            item.remove();
            this.items = this.items.filter(item => item.dataset.id !== id);
            // No listener cleanup needed
        }
        
        handleClick(id) {
            console.log('Delegated click:', id);
        }
    }
    
    console.log('Delegation advantage: Simplified management');
}

// DISADVANTAGES OF EVENT DELEGATION:

// 1. EVENT PROPAGATION DEPENDENCY
function propagationDisadvantage() {
    // Problem: Relies on event bubbling
    const container = document.getElementById('container');
    
    container.addEventListener('click', (e) => {
        if (e.target.matches('.target-element')) {
            console.log('Target clicked');
        }
    });
    
    // This can break delegation
    document.getElementById('problematic-child').addEventListener('click', (e) => {
        e.stopPropagation(); // Breaks delegation!
        console.log('Child clicked, but parent won\'t know');
    });
    
    console.log('Disadvantage: stopPropagation() breaks delegation');
}

// 2. SELECTOR COMPLEXITY
function selectorComplexity() {
    const container = document.getElementById('container');
    
    // Complex selectors can be hard to maintain
    container.addEventListener('click', (e) => {
        // This selector is getting complex and brittle
        if (e.target.matches('.card .header .actions .btn:not(.disabled)[data-action="edit"]')) {
            console.log('Complex selector matched');
        }
        
        // Multiple checks become unwieldy
        if (e.target.matches('.button') || 
            e.target.matches('.link') || 
            e.target.matches('.action-item')) {
            // Handle multiple types
        }
    });
    
    console.log('Disadvantage: Complex selectors hard to maintain');
}

// 3. PERFORMANCE WITH DEEP NESTING
function deepNestingDisadvantage() {
    // Problem: Every click bubbles through many levels
    const deeplyNested = document.getElementById('deeply-nested');
    
    // This listener fires for EVERY click in the container
    deeplyNested.addEventListener('click', (e) => {
        // Must check target for every click, even irrelevant ones
        if (e.target.matches('.deep-target')) {
            console.log('Deep target found');
        }
        // This function runs for every click, even on empty space
    });
    
    console.log('Disadvantage: Unnecessary function calls for irrelevant events');
}

// 4. DEBUGGING COMPLEXITY
function debuggingDisadvantage() {
    const container = document.getElementById('container');
    
    // Hard to debug - where is the actual handler?
    container.addEventListener('click', (e) => {
        if (e.target.matches('.type1')) {
            handleType1(e);
        } else if (e.target.matches('.type2')) {
            handleType2(e);
        } else if (e.target.matches('.type3')) {
            handleType3(e);
        }
        // Which handler will run? Hard to trace in dev tools
    });
    
    function handleType1(e) { console.log('Type 1'); }
    function handleType2(e) { console.log('Type 2'); }
    function handleType3(e) { console.log('Type 3'); }
    
    console.log('Disadvantage: Harder to debug than direct listeners');
}

// WHEN TO USE VS AVOID:

function usageGuidelines() {
    const guidelines = {
        useWhen: [
            'Large number of similar elements',
            'Dynamic content that changes frequently',
            'Performance is critical',
            'Simple event handling logic',
            'Elements have common parent'
        ],
        
        avoidWhen: [
            'Small number of elements (< 10)',
            'Complex, element-specific logic',
            'Events don\'t bubble (focus, blur, etc.)',
            'Deep event handling logic',
            'Need direct element reference frequently'
        ]
    };
    
    console.log('Usage guidelines:', guidelines);
}

// HYBRID APPROACH:
function hybridApproach() {
    // Combine delegation with direct listeners when appropriate
    
    const container = document.getElementById('hybrid-container');
    
    // Use delegation for simple, common actions
    container.addEventListener('click', (e) => {
        if (e.target.matches('.delete-btn')) {
            deleteItem(e.target.closest('.item'));
        } else if (e.target.matches('.toggle-btn')) {
            toggleItem(e.target.closest('.item'));
        }
    });
    
    // Use direct listeners for complex interactions
    const complexElements = document.querySelectorAll('.complex-widget');
    complexElements.forEach(element => {
        element.addEventListener('click', handleComplexInteraction);
        element.addEventListener('mouseenter', handleComplexHover);
        element.addEventListener('mouseleave', handleComplexUnhover);
    });
    
    function deleteItem(item) { item.remove(); }
    function toggleItem(item) { item.classList.toggle('active'); }
    function handleComplexInteraction(e) { /* Complex logic */ }
    function handleComplexHover(e) { /* Complex hover logic */ }
    function handleComplexUnhover(e) { /* Complex unhover logic */ }
    
    console.log('Hybrid approach: Use best tool for each situation');
}

performanceAdvantages();
dynamicContentAdvantage();
managementAdvantage();
propagationDisadvantage();
selectorComplexity();
deepNestingDisadvantage();
debuggingDisadvantage();
usageGuidelines();
hybridApproach();
```
</details>

**Intermediate: Q2** - How does event delegation work with dynamically added elements?

<details>
<summary>Answer</summary>

Event delegation automatically handles dynamically added elements because it listens on the parent, not individual elements:

```javascript
// HOW DELEGATION HANDLES DYNAMIC ELEMENTS:

// Traditional approach - BREAKS with dynamic content
function traditionalApproachProblem() {
    // Initial setup
    const initialButtons = document.querySelectorAll('.button');
    
    initialButtons.forEach(button => {
        button.addEventListener('click', () => {
            console.log('Button clicked:', button.textContent);
        });
    });
    
    // Problem: New buttons don't have listeners
    function addNewButton() {
        const button = document.createElement('button');
        button.className = 'button';
        button.textContent = 'New Button';
        document.body.appendChild(button);
        
        // This button won't respond to clicks!
        // Must manually add listener:
        button.addEventListener('click', () => {
            console.log('New button clicked:', button.textContent);
        });
    }
    
    setTimeout(addNewButton, 2000);
    console.log('Traditional: Must manually add listeners to new elements');
}

// Delegation approach - WORKS automatically
function delegationApproachSolution() {
    // Setup delegation once
    document.body.addEventListener('click', (e) => {
        if (e.target.classList.contains('delegated-button')) {
            console.log('Delegated button clicked:', e.target.textContent);
        }
    });
    
    // Add new buttons - they work automatically
    function addDelegatedButton(id) {
        const button = document.createElement('button');
        button.className = 'delegated-button';
        button.textContent = `Dynamic Button ${id}`;
        document.body.appendChild(button);
        
        // No need to add listener - delegation handles it!
    }
    
    // These buttons work immediately
    setTimeout(() => addDelegatedButton(1), 1000);
    setTimeout(() => addDelegatedButton(2), 2000);
    setTimeout(() => addDelegatedButton(3), 3000);
    
    console.log('Delegation: Dynamic elements work automatically');
}

// REAL-WORLD EXAMPLES:

// Example 1: Dynamic form fields
function dynamicFormFields() {
    const form = document.getElementById('dynamic-form');
    const addFieldBtn = document.getElementById('add-field-btn');
    let fieldCount = 0;
    
    // Delegation handles all form interactions
    form.addEventListener('input', (e) => {
        if (e.target.matches('input[type="text"]')) {
            validateField(e.target);
        } else if (e.target.matches('input[type="email"]')) {
            validateEmail(e.target);
        }
    });
    
    form.addEventListener('click', (e) => {
        if (e.target.matches('.remove-field')) {
            removeField(e.target.closest('.field-group'));
        }
    });
    
    // Add new fields dynamically
    addFieldBtn.addEventListener('click', () => {
        addFormField();
    });
    
    function addFormField() {
        fieldCount++;
        const fieldGroup = document.createElement('div');
        fieldGroup.className = 'field-group';
        fieldGroup.innerHTML = `
            <label>Field ${fieldCount}:</label>
            <input type="text" name="field${fieldCount}" required>
            <button type="button" class="remove-field">Remove</button>
        `;
        
        form.appendChild(fieldGroup);
        // Field automatically works with delegation
    }
    
    function validateField(input) {
        console.log('Validating field:', input.name);
        if (input.value.length < 3) {
            input.setCustomValidity('Minimum 3 characters');
        } else {
            input.setCustomValidity('');
        }
    }
    
    function validateEmail(input) {
        console.log('Validating email:', input.value);
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(input.value)) {
            input.setCustomValidity('Please enter a valid email');
        } else {
            input.setCustomValidity('');
        }
    }
    
    function removeField(fieldGroup) {
        fieldGroup.remove();
        console.log('Field removed');
    }
}

// Example 2: Dynamic todo list
function dynamicTodoList() {
    const todoList = document.getElementById('todo-list');
    const addTodoBtn = document.getElementById('add-todo-btn');
    const todoInput = document.getElementById('todo-input');
    let todoIdCounter = 0;
    
    // Delegation handles all todo interactions
    todoList.addEventListener('click', (e) => {
        const todoItem = e.target.closest('.todo-item');
        if (!todoItem) return;
        
        const todoId = todoItem.dataset.todoId;
        
        if (e.target.matches('.complete-btn')) {
            toggleComplete(todoId);
        } else if (e.target.matches('.edit-btn')) {
            enableEdit(todoId);
        } else if (e.target.matches('.delete-btn')) {
            deleteTodo(todoId);
        } else if (e.target.matches('.save-btn')) {
            saveEdit(todoId);
        }
    });
    
    todoList.addEventListener('keydown', (e) => {
        if (e.target.matches('.todo-text-input') && e.key === 'Enter') {
            saveEdit(e.target.closest('.todo-item').dataset.todoId);
        }
    });
    
    // Add new todos
    addTodoBtn.addEventListener('click', addTodo);
    todoInput.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') addTodo();
    });
    
    function addTodo() {
        const text = todoInput.value.trim();
        if (!text) return;
        
        todoIdCounter++;
        const todoItem = document.createElement('div');
        todoItem.className = 'todo-item';
        todoItem.dataset.todoId = todoIdCounter;
        todoItem.innerHTML = `
            <span class="todo-text">${text}</span>
            <div class="todo-actions">
                <button class="complete-btn">Complete</button>
                <button class="edit-btn">Edit</button>
                <button class="delete-btn">Delete</button>
            </div>
        `;
        
        todoList.appendChild(todoItem);
        todoInput.value = '';
        
        // Todo automatically works with delegation
        console.log('Todo added with delegation support');
    }
    
    function toggleComplete(todoId) {
        const todoItem = document.querySelector(`[data-todo-id="${todoId}"]`);
        todoItem.classList.toggle('completed');
        console.log('Todo completion toggled:', todoId);
    }
    
    function enableEdit(todoId) {
        const todoItem = document.querySelector(`[data-todo-id="${todoId}"]`);
        const textSpan = todoItem.querySelector('.todo-text');
        const currentText = textSpan.textContent;
        
        textSpan.innerHTML = `<input type="text" class="todo-text-input" value="${currentText}">`;
        todoItem.querySelector('.edit-btn').textContent = 'Save';
        todoItem.querySelector('.edit-btn').className = 'save-btn';
        
        console.log('Edit mode enabled:', todoId);
    }
    
    function saveEdit(todoId) {
        const todoItem = document.querySelector(`[data-todo-id="${todoId}"]`);
        const input = todoItem.querySelector('.todo-text-input');
        const newText = input.value.trim();
        
        if (newText) {
            todoItem.querySelector('.todo-text').textContent = newText;
            todoItem.querySelector('.save-btn').textContent = 'Edit';
            todoItem.querySelector('.save-btn').className = 'edit-btn';
            console.log('Todo updated:', todoId);
        }
    }
    
    function deleteTodo(todoId) {
        const todoItem = document.querySelector(`[data-todo-id="${todoId}"]`);
        todoItem.remove();
        console.log('Todo deleted:', todoId);
    }
}

// Example 3: Dynamic data table
function dynamicDataTable() {
    const table = document.getElementById('data-table');
    const addRowBtn = document.getElementById('add-row-btn');
    let rowIdCounter = 0;
    
    // Delegation handles all table interactions
    table.addEventListener('click', (e) => {
        const row = e.target.closest('tr[data-row-id]');
        if (!row) return;
        
        const rowId = row.dataset.rowId;
        
        if (e.target.matches('.edit-cell-btn')) {
            enableCellEdit(e.target.closest('td'));
        } else if (e.target.matches('.save-cell-btn')) {
            saveCellEdit(e.target.closest('td'));
        } else if (e.target.matches('.delete-row-btn')) {
            deleteRow(rowId);
        }
    });
    
    table.addEventListener('keydown', (e) => {
        if (e.target.matches('.cell-input') && e.key === 'Enter') {
            saveCellEdit(e.target.closest('td'));
        }
    });
    
    addRowBtn.addEventListener('click', addRow);
    
    function addRow() {
        rowIdCounter++;
        const row = document.createElement('tr');
        row.dataset.rowId = rowIdCounter;
        row.innerHTML = `
            <td>Item ${rowIdCounter} <button class="edit-cell-btn">Edit</button></td>
            <td>Description ${rowIdCounter} <button class="edit-cell-btn">Edit</button></td>
            <td>$${(Math.random() * 100).toFixed(2)} <button class="edit-cell-btn">Edit</button></td>
            <td><button class="delete-row-btn">Delete</button></td>
        `;
        
        table.querySelector('tbody').appendChild(row);
        console.log('Row added with delegation support');
    }
    
    function enableCellEdit(cell) {
        const text = cell.childNodes[0].textContent.trim();
        const editBtn = cell.querySelector('.edit-cell-btn');
        
        cell.innerHTML = `
            <input type="text" class="cell-input" value="${text}">
            <button class="save-cell-btn">Save</button>
        `;
    }
    
    function saveCellEdit(cell) {
        const input = cell.querySelector('.cell-input');
        const newValue = input.value.trim();
        
        cell.innerHTML = `${newValue} <button class="edit-cell-btn">Edit</button>`;
        console.log('Cell updated:', newValue);
    }
    
    function deleteRow(rowId) {
        document.querySelector(`tr[data-row-id="${rowId}"]`).remove();
        console.log('Row deleted:', rowId);
    }
}

// INFINITE SCROLL EXAMPLE:
function infiniteScrollWithDelegation() {
    const container = document.getElementById('infinite-container');
    let itemCount = 0;
    
    // Delegation handles all items, including future ones
    container.addEventListener('click', (e) => {
        if (e.target.matches('.like-btn')) {
            toggleLike(e.target.closest('.item'));
        } else if (e.target.matches('.share-btn')) {
            shareItem(e.target.closest('.item'));
        }
    });
    
    // Simulate infinite scroll
    function loadMoreItems() {
        for (let i = 0; i < 10; i++) {
            itemCount++;
            const item = document.createElement('div');
            item.className = 'item';
            item.dataset.itemId = itemCount;
            item.innerHTML = `
                <h3>Item ${itemCount}</h3>
                <p>Content for item ${itemCount}</p>
                <button class="like-btn">Like</button>
                <button class="share-btn">Share</button>
            `;
            
            container.appendChild(item);
        }
        
        console.log(`Loaded 10 more items (${itemCount} total) - all work with delegation`);
    }
    
    function toggleLike(item) {
        item.classList.toggle('liked');
        console.log('Like toggled for item:', item.dataset.itemId);
    }
    
    function shareItem(item) {
        console.log('Share item:', item.dataset.itemId);
    }
    
    // Initial load
    loadMoreItems();
    
    // Simulate scroll loading
    setInterval(loadMoreItems, 3000);
}

traditionalApproachProblem();
delegationApproachSolution();
dynamicFormFields();
dynamicTodoList();
dynamicDataTable();
infiniteScrollWithDelegation();
```
</details>

## Module patterns (CommonJS, ES Modules, AMD, UMD)

**Beginner: Q1** - What are the different module systems in JavaScript?

<details>
<summary>Answer</summary>

JavaScript has several module systems for organizing and sharing code across files:

```javascript
// 1. COMMONJS (Node.js default)
// Used in Node.js environments
// Synchronous loading

// Exporting in CommonJS
// math.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = { add, subtract };

// Alternative export syntax
exports.multiply = (a, b) => a * b;

// Importing in CommonJS
// app.js
const { add, subtract } = require('./math');
const math = require('./math');

console.log(add(2, 3)); // 5
console.log(math.subtract(5, 2)); // 3

// 2. ES MODULES (ES6/ES2015)
// Modern standard, supported in browsers and Node.js
// Static imports, tree-shaking friendly

// Exporting in ES Modules
// math.mjs or math.js (with type="module")
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

// Default export
export default function multiply(a, b) {
    return a * b;
}

// Named exports object
export { add as addition, subtract as subtraction };

// Importing in ES Modules
// app.mjs or app.js (with type="module")
import multiply, { add, subtract } from './math.js';
import { addition } from './math.js';
import * as mathUtils from './math.js';

console.log(add(2, 3)); // 5
console.log(multiply(4, 5)); // 20

// 3. AMD (Asynchronous Module Definition)
// Used in browsers with RequireJS
// Asynchronous loading

// Defining AMD module
// math.js
define(['dependency'], function(dep) {
    function add(a, b) {
        return a + b;
    }
    
    function subtract(a, b) {
        return a - b;
    }
    
    return {
        add: add,
        subtract: subtract
    };
});

// Using AMD module
// app.js
require(['math'], function(math) {
    console.log(math.add(2, 3)); // 5
});

// 4. UMD (Universal Module Definition)
// Works in CommonJS, AMD, and global environments
// Library compatibility pattern

// UMD module
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['exports'], factory);
    } else if (typeof exports === 'object' && typeof module !== 'undefined') {
        // CommonJS
        factory(exports);
    } else {
        // Browser global
        factory((root.math = {}));
    }
}(typeof self !== 'undefined' ? self : this, function (exports) {
    function add(a, b) {
        return a + b;
    }
    
    function subtract(a, b) {
        return a - b;
    }
    
    exports.add = add;
    exports.subtract = subtract;
}));

// 5. IIFE (Immediately Invoked Function Expression)
// Traditional browser pattern before modules

// IIFE module
var MathModule = (function() {
    // Private variables
    var privateVar = 'private';
    
    // Private functions
    function privateFunction() {
        return 'This is private';
    }
    
    // Public API
    return {
        add: function(a, b) {
            return a + b;
        },
        
        subtract: function(a, b) {
            return a - b;
        }
    };
})();

// Usage
console.log(MathModule.add(2, 3)); // 5

// COMPARISON SUMMARY:
const moduleComparison = {
    CommonJS: {
        environment: 'Node.js',
        loading: 'Synchronous',
        syntax: 'require/module.exports',
        treeShaking: 'No'
    },
    
    ESModules: {
        environment: 'Browser + Node.js',
        loading: 'Static analysis',
        syntax: 'import/export',
        treeShaking: 'Yes'
    },
    
    AMD: {
        environment: 'Browser (RequireJS)',
        loading: 'Asynchronous',
        syntax: 'define/require',
        treeShaking: 'No'
    },
    
    UMD: {
        environment: 'Universal',
        loading: 'Depends on environment',
        syntax: 'Multiple patterns',
        treeShaking: 'No'
    }
};

console.table(moduleComparison);
```
</details>

**Beginner: Q2** - How do you export and import in CommonJS?

<details>
<summary>Answer</summary>

CommonJS uses `module.exports`/`exports` for exporting and `require()` for importing:

```javascript
// EXPORTING IN COMMONJS:

// Method 1: module.exports with object
// utils.js
function formatDate(date) {
    return date.toISOString().split('T')[0];
}

function formatCurrency(amount) {
    return `$${amount.toFixed(2)}`;
}

module.exports = {
    formatDate,
    formatCurrency
};

// Method 2: module.exports with single value
// calculator.js
class Calculator {
    add(a, b) {
        return a + b;
    }
    
    subtract(a, b) {
        return a - b;
    }
}

module.exports = Calculator;

// Method 3: exports shorthand
// helpers.js
exports.isEmail = function(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

exports.isPhone = function(phone) {
    return /^\d{3}-\d{3}-\d{4}$/.test(phone);
};

// Method 4: Mixed exports
// api.js
const API_URL = 'https://api.example.com';

function get(endpoint) {
    return fetch(`${API_URL}/${endpoint}`);
}

function post(endpoint, data) {
    return fetch(`${API_URL}/${endpoint}`, {
        method: 'POST',
        body: JSON.stringify(data)
    });
}

// Export default function
module.exports = get;

// Add named exports
module.exports.get = get;
module.exports.post = post;
module.exports.API_URL = API_URL;

// IMPORTING IN COMMONJS:

// Method 1: Destructuring import
const { formatDate, formatCurrency } = require('./utils');

console.log(formatDate(new Date())); // 2023-10-17
console.log(formatCurrency(29.99)); // $29.99

// Method 2: Default import
const Calculator = require('./calculator');

const calc = new Calculator();
console.log(calc.add(5, 3)); // 8

// Method 3: Full module import
const helpers = require('./helpers');

console.log(helpers.isEmail('test@example.com')); // true
console.log(helpers.isPhone('123-456-7890')); // true

// Method 4: Mixed imports
const api = require('./api');
const { post, API_URL } = require('./api');

// Use default export
api('users').then(response => console.log('Users loaded'));

// Use named exports
post('users', { name: 'John' }).then(response => console.log('User created'));
console.log('API URL:', API_URL);

// ADVANCED PATTERNS:

// Conditional require
function loadModule(condition) {
    if (condition) {
        const module = require('./heavy-module');
        return module;
    }
    return null;
}

// Dynamic require
function requireByName(moduleName) {
    try {
        return require(moduleName);
    } catch (error) {
        console.log(`Module ${moduleName} not found`);
        return null;
    }
}

// Require with path resolution
const path = require('path');
const moduleFromPath = require(path.join(__dirname, 'modules', 'custom-module'));

// Caching behavior
console.log('First require');
const module1 = require('./cached-module');

console.log('Second require (cached)');
const module2 = require('./cached-module');

console.log(module1 === module2); // true - same instance

// Clear require cache
delete require.cache[require.resolve('./cached-module')];
const module3 = require('./cached-module'); // Fresh instance

// REAL-WORLD EXAMPLES:

// Example 1: Database module
// database.js
const mysql = require('mysql2/promise');

class Database {
    constructor(config) {
        this.config = config;
        this.connection = null;
    }
    
    async connect() {
        this.connection = await mysql.createConnection(this.config);
        console.log('Database connected');
    }
    
    async query(sql, params) {
        const [rows] = await this.connection.execute(sql, params);
        return rows;
    }
    
    async close() {
        await this.connection.end();
        console.log('Database disconnected');
    }
}

module.exports = Database;

// Example 2: Configuration module
// config.js
const fs = require('fs');
const path = require('path');

function loadConfig(env = 'development') {
    const configPath = path.join(__dirname, 'config', `${env}.json`);
    
    if (fs.existsSync(configPath)) {
        const configData = fs.readFileSync(configPath, 'utf8');
        return JSON.parse(configData);
    }
    
    throw new Error(`Configuration file not found: ${configPath}`);
}

module.exports = {
    loadConfig,
    database: loadConfig().database,
    server: loadConfig().server
};

// Example 3: Utility library
// lib/index.js
const stringUtils = require('./string-utils');
const dateUtils = require('./date-utils');
const arrayUtils = require('./array-utils');

module.exports = {
    string: stringUtils,
    date: dateUtils,
    array: arrayUtils
};

// Usage
// app.js
const Database = require('./database');
const config = require('./config');
const utils = require('./lib');

async function main() {
    const db = new Database(config.database);
    await db.connect();
    
    const users = await db.query('SELECT * FROM users');
    const formattedUsers = users.map(user => ({
        ...user,
        name: utils.string.capitalize(user.name),
        createdAt: utils.date.format(user.created_at)
    }));
    
    console.log('Formatted users:', formattedUsers);
    
    await db.close();
}

main().catch(console.error);

// ERROR HANDLING:
function safeRequire(modulePath) {
    try {
        return require(modulePath);
    } catch (error) {
        if (error.code === 'MODULE_NOT_FOUND') {
            console.error(`Module not found: ${modulePath}`);
            return null;
        }
        throw error; // Re-throw other errors
    }
}

// Optional dependencies
const optionalModule = safeRequire('./optional-module');
if (optionalModule) {
    console.log('Optional module loaded');
} else {
    console.log('Optional module not available, using fallback');
}
```
</details>

**Beginner: Q3** - What is the difference between CommonJS and ES Modules?

<details>
<summary>Answer</summary>

CommonJS and ES Modules differ in syntax, loading behavior, and environment support:

```javascript
// SYNTAX DIFFERENCES:

// CommonJS (require/module.exports)
// math-commonjs.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = { add, subtract };

// app-commonjs.js
const { add, subtract } = require('./math-commonjs');
const math = require('./math-commonjs');

// ES Modules (import/export)
// math-esm.js
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

// app-esm.js
import { add, subtract } from './math-esm.js';
import * as math from './math-esm.js';

// LOADING BEHAVIOR DIFFERENCES:

// CommonJS: Dynamic, runtime loading
function loadCommonJS() {
    // Can require conditionally
    if (process.env.NODE_ENV === 'development') {
        const devTools = require('./dev-tools'); // Runtime decision
        devTools.init();
    }
    
    // Can require in functions
    function loadWhenNeeded() {
        const heavyModule = require('./heavy-module'); // Loaded when function runs
        return heavyModule.process();
    }
    
    // Can build require path dynamically
    const moduleName = 'lodash';
    const dynamicRequire = require(moduleName); // Works
}

// ES Modules: Static, compile-time analysis
function loadESModules() {
    // Cannot import conditionally (this breaks)
    // if (condition) {
    //     import { something } from './module.js'; // ERROR: imports must be at top level
    // }
    
    // Cannot import in functions (without dynamic import)
    // function loadWhenNeeded() {
    //     import { something } from './module.js'; // ERROR
    // }
    
    // Can use dynamic import() for runtime loading
    async function dynamicLoad() {
        if (condition) {
            const module = await import('./module.js'); // Works with dynamic import
            module.something();
        }
    }
}

// HOISTING DIFFERENCES:

// CommonJS: No hoisting
console.log(add(2, 3)); // ERROR: add is not defined yet
const { add } = require('./math');

// ES Modules: Imports are hoisted
console.log(add(2, 3)); // Works! Import is hoisted
import { add } from './math.js';

// EXPORTS DIFFERENCES:

// CommonJS: Exports are objects, can be modified
// module1.js
module.exports = { value: 1 };

// app.js
const module1 = require('./module1');
module1.value = 2; // Modifies the exported object
module1.newProperty = 'added'; // Can add properties

// ES Modules: Exports are immutable bindings
// module2.js
export let value = 1;
export function increment() {
    value++; // This works - internal modification
}

// app.js
import { value, increment } from './module2.js';
// value = 2; // ERROR: Assignment to constant variable
console.log(value); // 1
increment();
console.log(value); // 2 - binding reflects internal changes

// TREE SHAKING DIFFERENCES:

// CommonJS: Cannot tree shake (full module imported)
const lodash = require('lodash'); // Entire library loaded
const isEmpty = lodash.isEmpty;

// ES Modules: Can tree shake (only used exports)
import { isEmpty } from 'lodash-es'; // Only isEmpty is bundled

// CIRCULAR DEPENDENCIES:

// CommonJS: Partial exports during cycles
// a.js
console.log('a starting');
exports.done = false;
const b = require('./b.js');
console.log('in a, b.done =', b.done);
exports.done = true;
console.log('a done');

// b.js
console.log('b starting');
exports.done = false;
const a = require('./a.js');
console.log('in b, a.done =', a.done);
exports.done = true;
console.log('b done');

// ES Modules: Better circular dependency handling
// a.mjs
console.log('a starting');
import { bValue } from './b.mjs';
export const aValue = 'a';
console.log('in a, bValue =', bValue);

// b.mjs
console.log('b starting');
import { aValue } from './a.mjs';
export const bValue = 'b';
console.log('in b, aValue =', aValue);

// ENVIRONMENT SUPPORT:

// CommonJS
const environmentSupport = {
    commonjs: {
        nodejs: 'Native support',
        browser: 'Requires bundler (webpack, browserify)',
        support: 'Universal with tooling'
    },
    
    esmodules: {
        nodejs: 'Support since v12 (with .mjs or "type": "module")',
        browser: 'Native support in modern browsers',
        support: 'Growing native support'
    }
};

// FILE EXTENSIONS:

// CommonJS
// .js files (default in Node.js)
// No special extension needed

// ES Modules in Node.js
// .mjs files, or
// .js files with "type": "module" in package.json

// PERFORMANCE:

// CommonJS: Runtime overhead
const performanceCommonJS = {
    loading: 'Synchronous, blocking',
    parsing: 'Runtime execution required',
    optimization: 'Limited bundler optimizations'
};

// ES Modules: Better performance
const performanceESM = {
    loading: 'Can be asynchronous',
    parsing: 'Static analysis possible',
    optimization: 'Tree shaking, better bundling'
};

// MIGRATION EXAMPLE:

// Converting CommonJS to ES Modules
// Before (CommonJS)
// utils.js
const fs = require('fs');
const path = require('path');

function readConfigFile(filename) {
    const filePath = path.join(__dirname, filename);
    return JSON.parse(fs.readFileSync(filePath, 'utf8'));
}

module.exports = { readConfigFile };

// After (ES Modules)
// utils.mjs
import fs from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __dirname = path.dirname(fileURLToPath(import.meta.url));

export function readConfigFile(filename) {
    const filePath = path.join(__dirname, filename);
    return JSON.parse(fs.readFileSync(filePath, 'utf8'));
}

// INTEROPERABILITY:

// Using CommonJS in ES Modules
import util from 'util'; // CommonJS module
import { promisify } from 'util'; // Named import from CommonJS

// Using ES Modules in CommonJS (dynamic import)
async function useESModule() {
    const { someFunction } = await import('./es-module.mjs');
    return someFunction();
}

// COMPARISON TABLE:
const comparison = {
    feature: {
        commonjs: 'CommonJS',
        esmodules: 'ES Modules'
    },
    syntax: {
        commonjs: 'require/module.exports',
        esmodules: 'import/export'
    },
    loading: {
        commonjs: 'Dynamic, runtime',
        esmodules: 'Static, compile-time'
    },
    hoisting: {
        commonjs: 'No',
        esmodules: 'Yes'
    },
    treeShaking: {
        commonjs: 'No',
        esmodules: 'Yes'
    },
    environment: {
        commonjs: 'Node.js native',
        esmodules: 'Browser + Node.js'
    }
};

console.table(comparison);
```
</details>

**Intermediate: Q1** - What is UMD and why was it created?

<details>
<summary>Answer</summary>

UMD (Universal Module Definition) is a pattern that makes modules work across different environments (CommonJS, AMD, and global):

```javascript
// WHAT IS UMD:
// UMD = Universal Module Definition
// A pattern that detects the environment and adapts accordingly

// BASIC UMD PATTERN:
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD environment (RequireJS)
        define(['dependency'], factory);
    } else if (typeof module === 'object' && module.exports) {
        // CommonJS environment (Node.js)
        module.exports = factory(require('dependency'));
    } else {
        // Browser global environment
        root.MyLibrary = factory(root.Dependency);
    }
}(typeof self !== 'undefined' ? self : this, function (dependency) {
    // Module code here
    function MyLibrary() {
        // Library implementation
    }
    
    return MyLibrary;
}));

// WHY UMD WAS CREATED:

// Problem 1: Module system fragmentation
function demonstrateFragmentation() {
    // Before UMD, libraries needed separate builds:
    
    // For CommonJS (Node.js)
    // library-commonjs.js
    const dependency = require('dependency');
    function MyLibrary() { /* ... */ }
    module.exports = MyLibrary;
    
    // For AMD (RequireJS)
    // library-amd.js
    define(['dependency'], function(dependency) {
        function MyLibrary() { /* ... */ }
        return MyLibrary;
    });
    
    // For browsers (global)
    // library-global.js
    (function(global, dependency) {
        function MyLibrary() { /* ... */ }
        global.MyLibrary = MyLibrary;
    })(window, window.Dependency);
    
    console.log('Problem: Multiple builds for different environments');
}

// Problem 2: Library maintenance complexity
function maintenanceComplexity() {
    const problems = [
        'Multiple build scripts',
        'Separate test suites',
        'Different packaging',
        'Version synchronization',
        'Documentation for each format'
    ];
    
    console.log('Maintenance problems before UMD:', problems);
}

// COMPREHENSIVE UMD PATTERNS:

// Pattern 1: Simple UMD (no dependencies)
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory();
    } else {
        root.SimpleLibrary = factory();
    }
}(typeof self !== 'undefined' ? self : this, function () {
    'use strict';
    
    function SimpleLibrary() {
        this.version = '1.0.0';
    }
    
    SimpleLibrary.prototype.greet = function(name) {
        return `Hello, ${name}!`;
    };
    
    return SimpleLibrary;
}));

// Pattern 2: UMD with dependencies
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery', 'underscore'], factory);
    } else if (typeof exports === 'object') {
        // CommonJS
        module.exports = factory(require('jquery'), require('underscore'));
    } else {
        // Browser globals
        root.MyPlugin = factory(root.jQuery, root._);
    }
}(typeof self !== 'undefined' ? self : this, function ($, _) {
    'use strict';
    
    function MyPlugin(options) {
        this.options = _.defaults(options || {}, {
            selector: '.my-plugin',
            animation: 'fade'
        });
        
        this.init();
    }
    
    MyPlugin.prototype.init = function() {
        const self = this;
        $(this.options.selector).on('click', function() {
            self.animate(this);
        });
    };
    
    MyPlugin.prototype.animate = function(element) {
        $(element).fadeToggle();
    };
    
    return MyPlugin;
}));

// Pattern 3: UMD with conditional dependencies
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define(['jquery'], function($) {
            return factory(root, $);
        });
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory(root, require('jquery'));
    } else {
        root.ConditionalLibrary = factory(root, root.jQuery);
    }
}(typeof self !== 'undefined' ? self : this, function (root, $) {
    'use strict';
    
    function ConditionalLibrary() {
        // Use jQuery if available, otherwise vanilla JS
        this.$ = $ || null;
    }
    
    ConditionalLibrary.prototype.select = function(selector) {
        if (this.$) {
            return this.$(selector);
        } else {
            return document.querySelectorAll(selector);
        }
    };
    
    return ConditionalLibrary;
}));

// REAL-WORLD UMD EXAMPLES:

// Example 1: Math utility library
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory();
    } else {
        root.MathUtils = factory();
    }
}(typeof self !== 'undefined' ? self : this, function () {
    'use strict';
    
    const MathUtils = {
        add: function(a, b) {
            return a + b;
        },
        
        subtract: function(a, b) {
            return a - b;
        },
        
        multiply: function(a, b) {
            return a * b;
        },
        
        divide: function(a, b) {
            if (b === 0) throw new Error('Division by zero');
            return a / b;
        },
        
        // Utility methods
        isEven: function(num) {
            return num % 2 === 0;
        },
        
        factorial: function(n) {
            if (n < 0) throw new Error('Negative number');
            if (n === 0 || n === 1) return 1;
            return n * this.factorial(n - 1);
        }
    };
    
    return MathUtils;
}));

// Example 2: Event emitter library
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory();
    } else {
        root.EventEmitter = factory();
    }
}(typeof self !== 'undefined' ? self : this, function () {
    'use strict';
    
    function EventEmitter() {
        this.events = {};
    }
    
    EventEmitter.prototype.on = function(event, listener) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(listener);
        return this;
    };
    
    EventEmitter.prototype.emit = function(event, ...args) {
        if (!this.events[event]) return false;
        
        this.events[event].forEach(listener => {
            listener.apply(this, args);
        });
        
        return true;
    };
    
    EventEmitter.prototype.off = function(event, listenerToRemove) {
        if (!this.events[event]) return this;
        
        if (!listenerToRemove) {
            delete this.events[event];
        } else {
            this.events[event] = this.events[event].filter(
                listener => listener !== listenerToRemove
            );
        }
        
        return this;
    };
    
    return EventEmitter;
}));

// USAGE IN DIFFERENT ENVIRONMENTS:

// In Node.js (CommonJS)
function nodeUsage() {
    const MathUtils = require('./math-utils-umd');
    console.log(MathUtils.add(2, 3)); // 5
    console.log(MathUtils.factorial(5)); // 120
}

// In RequireJS (AMD)
function amdUsage() {
    require(['math-utils-umd'], function(MathUtils) {
        console.log(MathUtils.add(2, 3)); // 5
        console.log(MathUtils.factorial(5)); // 120
    });
}

// In browser (global)
function browserUsage() {
    // <script src="math-utils-umd.js"></script>
    console.log(window.MathUtils.add(2, 3)); // 5
    console.log(window.MathUtils.factorial(5)); // 120
}

// MODERN ALTERNATIVES TO UMD:

// 1. ES Modules with build tools
function modernApproach() {
    // math-utils.js (ES Module)
    export function add(a, b) {
        return a + b;
    }
    
    export function subtract(a, b) {
        return a - b;
    }
    
    // Build tools (webpack, rollup) can generate UMD if needed
    // webpack.config.js
    const config = {
        output: {
            library: 'MathUtils',
            libraryTarget: 'umd',
            globalObject: 'typeof self !== \'undefined\' ? self : this'
        }
    };
}

// 2. Conditional exports (package.json)
function conditionalExports() {
    // package.json
    const packageJson = {
        "name": "my-library",
        "version": "1.0.0",
        "main": "./lib/index.cjs",    // CommonJS
        "module": "./lib/index.mjs",  // ES Module
        "browser": "./lib/index.umd.js", // UMD for browsers
        "exports": {
            ".": {
                "import": "./lib/index.mjs",
                "require": "./lib/index.cjs",
                "browser": "./lib/index.umd.js"
            }
        }
    };
}

// UMD GENERATOR UTILITY:
function createUMD(name, dependencies, factory) {
    const dependencyNames = dependencies.map(dep => dep.name);
    const dependencyPaths = dependencies.map(dep => dep.path);
    const dependencyGlobals = dependencies.map(dep => dep.global);
    
    return `
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([${dependencyPaths.map(p => `'${p}'`).join(', ')}], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory(${dependencyPaths.map(p => `require('${p}')`).join(', ')});
    } else {
        root.${name} = factory(${dependencyGlobals.map(g => `root.${g}`).join(', ')});
    }
}(typeof self !== 'undefined' ? self : this, function (${dependencyNames.join(', ')}) {
    'use strict';
    
    ${factory.toString()}
    
    return ${name};
}));
    `;
}

// WHEN TO USE UMD TODAY:
const umdUseCases = {
    libraryDevelopment: 'Creating libraries for maximum compatibility',
    legacySupport: 'Supporting older environments without ES modules',
    standaloneScripts: 'Scripts that work without build tools',
    backwards compatibility: 'Maintaining compatibility with existing code'
};

const modernAlternatives = {
    esBuild: 'Use ES modules with build tools',
    conditionalExports: 'Use package.json exports field',
    dualPackaging: 'Publish both ES and CommonJS versions',
    polyfills: 'Use polyfills for ES module support'
};

console.log('UMD use cases:', umdUseCases);
console.log('Modern alternatives:', modernAlternatives);

demonstrateFragmentation();
maintenanceComplexity();
```
</details>

**Intermediate: Q2** - How do you create a module that works in both Node.js and browsers?

<details>
<summary>Answer</summary>

Create universal modules using UMD pattern, build tools, or modern dual packaging approaches:

```javascript
// METHOD 1: UMD PATTERN (Universal)

// universal-module.js
(function (root, factory) {
    // Detect environment and adapt
    if (typeof define === 'function' && define.amd) {
        // AMD (RequireJS) environment
        define(['dependency'], factory);
    } else if (typeof module === 'object' && module.exports) {
        // CommonJS (Node.js) environment
        module.exports = factory(require('dependency'));
    } else {
        // Browser global environment
        root.UniversalModule = factory(root.Dependency);
    }
}(typeof self !== 'undefined' ? self : this, function (dependency) {
    'use strict';
    
    // Universal module implementation
    function UniversalModule(options) {
        this.options = Object.assign({
            debug: false,
            timeout: 5000
        }, options);
    }
    
    UniversalModule.prototype.init = function() {
        if (this.options.debug) {
            console.log('UniversalModule initialized');
        }
    };
    
    UniversalModule.prototype.fetch = function(url) {
        // Environment-specific implementations
        if (typeof window !== 'undefined' && window.fetch) {
            // Browser fetch
            return window.fetch(url);
        } else if (typeof require === 'function') {
            // Node.js with dynamic require
            try {
                const https = require('https');
                const http = require('http');
                
                return new Promise((resolve, reject) => {
                    const protocol = url.startsWith('https:') ? https : http;
                    protocol.get(url, (res) => {
                        let data = '';
                        res.on('data', chunk => data += chunk);
                        res.on('end', () => resolve({ text: () => Promise.resolve(data) }));
                        res.on('error', reject);
                    });
                });
            } catch (error) {
                return Promise.reject(error);
            }
        } else {
            return Promise.reject(new Error('No fetch implementation available'));
        }
    };
    
    return UniversalModule;
}));

// METHOD 2: CONDITIONAL ENVIRONMENT DETECTION

// environment-aware.js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory();
    } else {
        root.EnvironmentAware = factory();
    }
}(typeof self !== 'undefined' ? self : this, function () {
    'use strict';
    
    // Detect environment
    const isNode = typeof process !== 'undefined' && 
                   process.versions && 
                   process.versions.node;
    
    const isBrowser = typeof window !== 'undefined' && 
                      typeof document !== 'undefined';
    
    const isWebWorker = typeof importScripts === 'function';
    
    function EnvironmentAware() {
        this.environment = this.detectEnvironment();
    }
    
    EnvironmentAware.prototype.detectEnvironment = function() {
        if (isNode) return 'node';
        if (isWebWorker) return 'webworker';
        if (isBrowser) return 'browser';
        return 'unknown';
    };
    
    // Environment-specific storage
    EnvironmentAware.prototype.getStorage = function() {
        switch (this.environment) {
            case 'node':
                // Use filesystem
                const fs = require('fs');
                return {
                    set: (key, value) => {
                        fs.writeFileSync(`${key}.json`, JSON.stringify(value));
                    },
                    get: (key) => {
                        try {
                            return JSON.parse(fs.readFileSync(`${key}.json`, 'utf8'));
                        } catch (e) {
                            return null;
                        }
                    }
                };
                
            case 'browser':
                // Use localStorage
                return {
                    set: (key, value) => {
                        localStorage.setItem(key, JSON.stringify(value));
                    },
                    get: (key) => {
                        try {
                            return JSON.parse(localStorage.getItem(key));
                        } catch (e) {
                            return null;
                        }
                    }
                };
                
            case 'webworker':
                // Use in-memory storage
                const storage = {};
                return {
                    set: (key, value) => {
                        storage[key] = value;
                    },
                    get: (key) => {
                        return storage[key] || null;
                    }
                };
                
            default:
                throw new Error('Unsupported environment');
        }
    };
    
    return EnvironmentAware;
}));

// METHOD 3: BUILD TOOL APPROACH (Webpack/Rollup)

// Source: universal-logger.js (ES Module)
class UniversalLogger {
    constructor(options = {}) {
        this.level = options.level || 'info';
        this.timestamp = options.timestamp !== false;
    }
    
    log(level, message, ...args) {
        if (!this.shouldLog(level)) return;
        
        const timestamp = this.timestamp ? new Date().toISOString() : '';
        const prefix = timestamp ? `[${timestamp}] ` : '';
        
        // Environment-specific output
        if (typeof console !== 'undefined') {
            console[level] && console[level](`${prefix}${message}`, ...args);
        }
    }
    
    shouldLog(level) {
        const levels = ['error', 'warn', 'info', 'debug'];
        return levels.indexOf(level) <= levels.indexOf(this.level);
    }
    
    error(message, ...args) { this.log('error', message, ...args); }
    warn(message, ...args) { this.log('warn', message, ...args); }
    info(message, ...args) { this.log('info', message, ...args); }
    debug(message, ...args) { this.log('debug', message, ...args); }
}

export default UniversalLogger;

// Webpack config for universal build
const webpackConfig = {
    entry: './src/universal-logger.js',
    output: {
        filename: 'universal-logger.js',
        library: 'UniversalLogger',
        libraryTarget: 'umd',
        globalObject: 'typeof self !== \'undefined\' ? self : this'
    },
    mode: 'production',
    target: ['web', 'node']
};

// METHOD 4: DUAL PACKAGING (Modern Approach)

// Package structure:
/*
my-universal-package/
├── package.json
├── src/
│   └── index.js (ES Module source)
├── lib/
│   ├── index.cjs (CommonJS build)
│   ├── index.mjs (ES Module build)
│   └── index.umd.js (UMD build)
└── types/
    └── index.d.ts (TypeScript definitions)
*/

// package.json with conditional exports
const packageJsonConfig = {
    "name": "my-universal-package",
    "version": "1.0.0",
    "description": "Universal package that works everywhere",
    "main": "./lib/index.cjs",        // CommonJS entry
    "module": "./lib/index.mjs",      // ES Module entry
    "browser": "./lib/index.umd.js",  // Browser UMD entry
    "types": "./types/index.d.ts",    // TypeScript definitions
    "exports": {
        ".": {
            "import": "./lib/index.mjs",    // ES Module import
            "require": "./lib/index.cjs",   // CommonJS require
            "browser": "./lib/index.umd.js" // Browser script tag
        }
    },
    "scripts": {
        "build": "npm run build:cjs && npm run build:esm && npm run build:umd",
        "build:cjs": "babel src --out-dir lib --presets @babel/preset-env",
        "build:esm": "babel src --out-dir lib --keep-file-extension",
        "build:umd": "webpack --config webpack.config.js"
    }
};

// METHOD 5: FEATURE DETECTION APPROACH

// feature-based-module.js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        module.exports = factory();
    } else {
        root.FeatureBasedModule = factory();
    }
}(typeof self !== 'undefined' ? self : this, function () {
    'use strict';
    
    function FeatureBasedModule() {
        this.features = this.detectFeatures();
    }
    
    FeatureBasedModule.prototype.detectFeatures = function() {
        return {
            fetch: typeof fetch !== 'undefined',
            promise: typeof Promise !== 'undefined',
            localStorage: typeof localStorage !== 'undefined',
            fileSystem: typeof require === 'function' && (() => {
                try {
                    require('fs');
                    return true;
                } catch (e) {
                    return false;
                }
            })(),
            webWorker: typeof importScripts === 'function',
            crypto: typeof crypto !== 'undefined'
        };
    };
    
    FeatureBasedModule.prototype.request = function(url, options) {
        if (this.features.fetch) {
            return fetch(url, options);
        } else if (this.features.fileSystem) {
            // Node.js implementation
            const https = require('https');
            const http = require('http');
            
            return new Promise((resolve, reject) => {
                const protocol = url.startsWith('https:') ? https : http;
                const req = protocol.request(url, options, (res) => {
                    let data = '';
                    res.on('data', chunk => data += chunk);
                    res.on('end', () => resolve({ 
                        text: () => Promise.resolve(data),
                        json: () => Promise.resolve(JSON.parse(data))
                    }));
                });
                req.on('error', reject);
                req.end();
            });
        } else {
            throw new Error('No HTTP implementation available');
        }
    };
    
    FeatureBasedModule.prototype.store = function(key, value) {
        if (this.features.localStorage) {
            localStorage.setItem(key, JSON.stringify(value));
        } else if (this.features.fileSystem) {
            const fs = require('fs');
            fs.writeFileSync(`${key}.json`, JSON.stringify(value));
        } else {
            // In-memory fallback
            this._storage = this._storage || {};
            this._storage[key] = value;
        }
    };
    
    FeatureBasedModule.prototype.retrieve = function(key) {
        if (this.features.localStorage) {
            try {
                return JSON.parse(localStorage.getItem(key));
            } catch (e) {
                return null;
            }
        } else if (this.features.fileSystem) {
            try {
                const fs = require('fs');
                return JSON.parse(fs.readFileSync(`${key}.json`, 'utf8'));
            } catch (e) {
                return null;
            }
        } else {
            return (this._storage && this._storage[key]) || null;
        }
    };
    
    return FeatureBasedModule;
}));

// USAGE EXAMPLES:

// In Node.js
function nodeUsage() {
    const UniversalModule = require('./universal-module');
    const module = new UniversalModule({ debug: true });
    
    module.init();
    module.fetch('https://api.example.com/data')
        .then(response => response.text())
        .then(data => console.log(data));
}

// In browser
function browserUsage() {
    // <script src="universal-module.js"></script>
    const module = new UniversalModule({ debug: true });
    
    module.init();
    module.fetch('/api/data')
        .then(response => response.text())
        .then(data => console.log(data));
}

// In RequireJS
function amdUsage() {
    require(['universal-module'], function(UniversalModule) {
        const module = new UniversalModule({ debug: true });
        module.init();
    });
}

// TESTING UNIVERSAL MODULES:

// test-universal.js
function testUniversalModule() {
    // Environment detection
    const isNode = typeof process !== 'undefined';
    const isBrowser = typeof window !== 'undefined';
    
    if (isNode) {
        // Node.js tests
        const UniversalModule = require('./universal-module');
        const assert = require('assert');
        
        const module = new UniversalModule();
        assert(module instanceof UniversalModule);
        console.log('Node.js tests passed');
    }
    
    if (isBrowser) {
        // Browser tests
        const module = new UniversalModule();
        console.assert(module instanceof UniversalModule);
        console.log('Browser tests passed');
    }
}

// Modern development approach
const modernDevelopment = {
    source: 'Write in ES modules',
    build: 'Use build tools (webpack/rollup) to generate multiple formats',
    distribute: 'Publish with conditional exports in package.json',
    test: 'Test in all target environments',
    maintain: 'Single source of truth with automated builds'
};

console.log('Modern universal module development:', modernDevelopment);
```
</details>

## DOM manipulation (querySelector, createElement, etc.)

**Beginner: Q1** - How do you select elements from the DOM?

<details>
<summary>Answer</summary>

Use `querySelector()`, `getElementById()`, and related methods:

```javascript
// Single element
document.getElementById('myId');
document.querySelector('.class');
document.querySelector('#id');

// Multiple elements
document.querySelectorAll('.class');
document.getElementsByClassName('class');
document.getElementsByTagName('div');
```
</details>

**Beginner: Q2** - How do you create and append new elements to the DOM?

<details>
<summary>Answer</summary>

Use `createElement()` and `appendChild()`:

```javascript
const div = document.createElement('div');
div.textContent = 'Hello World';
div.className = 'my-class';

document.body.appendChild(div);
// or
document.getElementById('container').appendChild(div);
```
</details>

**Beginner: Q3** - What is the difference between `innerHTML`, `innerText`, and `textContent`?

<details>
<summary>Answer</summary>

- `innerHTML`: Gets/sets HTML content including tags
- `innerText`: Gets/sets visible text only (respects styling)
- `textContent`: Gets/sets all text content (ignores styling)

```javascript
const div = document.querySelector('div');
div.innerHTML = '<b>Bold</b>'; // Renders bold text
div.innerText = '<b>Bold</b>'; // Shows literal <b>Bold</b>
div.textContent = '<b>Bold</b>'; // Shows literal <b>Bold</b>
```
</details>

**Intermediate: Q1** - How do you efficiently manipulate multiple DOM elements?

<details>
<summary>Answer</summary>

Use `DocumentFragment` or batch operations to minimize reflows:

```javascript
// DocumentFragment
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
    const div = document.createElement('div');
    fragment.appendChild(div);
}
document.body.appendChild(fragment); // Single reflow

// Batch with CSS
element.style.display = 'none'; // Batch changes
element.style.width = '100px';
element.style.display = 'block';
```
</details>

**Intermediate: Q2** - What is the difference between `querySelector` and `getElementById` in terms of performance?

<details>
<summary>Answer</summary>

`getElementById` is faster than `querySelector`:

- `getElementById`: Direct hash table lookup, optimized by browsers
- `querySelector`: Must parse CSS selector, more flexible but slower

```javascript
// Faster
document.getElementById('myId');

// Slower but more flexible
document.querySelector('#myId');
document.querySelector('.class[data-attr]');
```

Use `getElementById` when selecting by ID only.
</details>

## Event bubbling & capturing

**Beginner: Q1** - What is event bubbling?

<details>
<summary>Answer</summary>

Event bubbling is when events propagate from the target element up to the root:

```javascript
// HTML: <div id="parent"><button id="child">Click</button></div>

document.getElementById('parent').addEventListener('click', () => 
    console.log('Parent'));
document.getElementById('child').addEventListener('click', () => 
    console.log('Child'));

// Click button logs: "Child" then "Parent"
```
</details>

**Beginner: Q2** - What is event capturing?

<details>
<summary>Answer</summary>

Event capturing propagates from root down to the target element. Use third parameter `true`:

```javascript
document.getElementById('parent').addEventListener('click', () => 
    console.log('Parent'), true); // true = capturing
document.getElementById('child').addEventListener('click', () => 
    console.log('Child'));

// Click button logs: "Parent" then "Child"
```
</details>

**Beginner: Q3** - How do you stop event bubbling?

<details>
<summary>Answer</summary>

Use `event.stopPropagation()`:

```javascript
document.getElementById('child').addEventListener('click', (e) => {
    console.log('Child');
    e.stopPropagation(); // Stops bubbling to parent
});

document.getElementById('parent').addEventListener('click', () => 
    console.log('Parent')); // Won't trigger
```
</details>

**Intermediate: Q1** - What will happen in this code?
```javascript
document.body.addEventListener('click', () => console.log('body'), true);
document.getElementById('button').addEventListener('click', () => console.log('button'));
// User clicks the button
```

<details>
<summary>Answer</summary>

Output: "body" then "button"

The body listener uses capturing (third parameter `true`), so it fires first during the capturing phase. Then the button listener fires during the bubbling phase.
</details>

**Intermediate: Q2** - What is the difference between `stopPropagation()` and `stopImmediatePropagation()`?

<details>
<summary>Answer</summary>

- `stopPropagation()`: Stops event from bubbling/capturing to other elements
- `stopImmediatePropagation()`: Stops propagation AND prevents other listeners on the same element

```javascript
element.addEventListener('click', (e) => {
    e.stopImmediatePropagation();
    console.log('First'); // Runs
});

element.addEventListener('click', () => {
    console.log('Second'); // Won't run
});
```
</details>

## Event listeners & passive events

**Beginner: Q1** - How do you add and remove event listeners?

<details>
<summary>Answer</summary>

Use `addEventListener()` and `removeEventListener()`:

```javascript
// Add listener
function handleClick() {
    console.log('Clicked');
}
element.addEventListener('click', handleClick);

// Remove listener (must use same function reference)
element.removeEventListener('click', handleClick);

// With options
element.addEventListener('click', handleClick, { once: true });
```
</details>

**Beginner: Q2** - What is the difference between `addEventListener` and inline event handlers?

<details>
<summary>Answer</summary>

- `addEventListener`: Can attach multiple listeners, better separation, more options
- Inline handlers: Only one per event, mixed with HTML

```javascript
// addEventListener (preferred)
element.addEventListener('click', handler1);
element.addEventListener('click', handler2); // Both work

// Inline (overwrites previous)
element.onclick = handler1;
element.onclick = handler2; // Only handler2 works
```
</details>

**Beginner: Q3** - Can you add multiple event listeners to the same element?

<details>
<summary>Answer</summary>

Yes, with `addEventListener()`:

```javascript
element.addEventListener('click', () => console.log('First'));
element.addEventListener('click', () => console.log('Second'));
element.addEventListener('click', () => console.log('Third'));

// All three will execute when clicked
```
</details>

**Intermediate: Q1** - What are passive event listeners and when would you use them?

<details>
<summary>Answer</summary>

Passive listeners promise not to call `preventDefault()`, allowing browser optimizations:

```javascript
// Passive listener (can't prevent default)
element.addEventListener('touchstart', handler, { passive: true });

// Use for:
// - Scroll events
// - Touch events
// - When you don't need preventDefault()

// Benefits: Better scroll performance
```
</details>

**Intermediate: Q2** - How do you prevent the default behavior of an event?

<details>
<summary>Answer</summary>

Use `event.preventDefault()`:

```javascript
// Prevent form submission
form.addEventListener('submit', (e) => {
    e.preventDefault();
    // Custom validation
});

// Prevent link navigation
link.addEventListener('click', (e) => {
    e.preventDefault();
    // Custom action
});
```
</details>

## LocalStorage, SessionStorage, Cookies

**Beginner: Q1** - What are the differences between localStorage, sessionStorage, and cookies?

<details>
<summary>Answer</summary>

| Feature | localStorage | sessionStorage | Cookies |
|---------|-------------|----------------|---------|
| Persistence | Until cleared | Until tab closed | Set expiration |
| Storage | 5-10MB | 5-10MB | 4KB |
| Server Access | No | No | Yes (auto-sent) |
| Scope | Domain | Tab/window | Domain |

```javascript
localStorage.setItem('key', 'value'); // Persistent
sessionStorage.setItem('key', 'value'); // Tab session
document.cookie = 'key=value'; // Can expire
```
</details>

**Beginner: Q2** - How do you store and retrieve data from localStorage?

<details>
<summary>Answer</summary>

Use `setItem()` and `getItem()`:

```javascript
// Store data
localStorage.setItem('username', 'john');
localStorage.setItem('settings', JSON.stringify({theme: 'dark'}));

// Retrieve data
const username = localStorage.getItem('username');
const settings = JSON.parse(localStorage.getItem('settings'));

// Remove data
localStorage.removeItem('username');
localStorage.clear(); // Remove all
```
</details>

**Beginner: Q3** - What happens to sessionStorage when the browser tab is closed?

<details>
<summary>Answer</summary>

sessionStorage is completely cleared when the tab is closed. Data is only available during the browser session and is isolated per tab/window.

```javascript
// This data disappears when tab closes
sessionStorage.setItem('tempData', 'value');

// localStorage persists across browser restarts
localStorage.setItem('persistentData', 'value');
```
</details>

**Intermediate: Q1** - What are the storage limits for each storage mechanism?

<details>
<summary>Answer</summary>

- **localStorage/sessionStorage**: 5-10MB per origin (browser-dependent)
- **Cookies**: 4KB per cookie, ~50 cookies per domain
- **IndexedDB**: Much larger (hundreds of MB to GB)

```javascript
// Check available storage
if (navigator.storage && navigator.storage.estimate) {
    navigator.storage.estimate().then(estimate => {
        console.log(`Used: ${estimate.usage}, Quota: ${estimate.quota}`);
    });
}
```
</details>

**Intermediate: Q2** - How do you handle storage events and synchronize data across tabs?

<details>
<summary>Answer</summary>

Use the `storage` event to sync across tabs:

```javascript
// Listen for storage changes
window.addEventListener('storage', (e) => {
    if (e.key === 'username') {
        console.log(`Username changed from ${e.oldValue} to ${e.newValue}`);
        updateUI(e.newValue);
    }
});

// Storage event only fires in OTHER tabs, not the one making changes
localStorage.setItem('username', 'newUser');
```
</details>

## Fetch API / XMLHttpRequest

**Beginner: Q1** - How do you make an HTTP request using the Fetch API?

<details>
<summary>Answer</summary>

Use `fetch()` with URL and optional options:

```javascript
// Basic GET request
fetch('https://api.example.com/users')
    .then(response => response.json())
    .then(data => console.log(data));

// With options
fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({name: 'John'})
});
```
</details>

**Beginner: Q2** - How do you handle the response from a fetch request?

<details>
<summary>Answer</summary>

Chain `.then()` to process the response:

```javascript
fetch('/api/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json(); // or .text(), .blob()
    })
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));

// Async/await version
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```
</details>

**Beginner: Q3** - What is the difference between Fetch API and XMLHttpRequest?

<details>
<summary>Answer</summary>

- **Fetch**: Modern, Promise-based, cleaner syntax
- **XMLHttpRequest**: Older, callback-based, more verbose

```javascript
// Fetch (modern)
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data));

// XMLHttpRequest (older)
const xhr = new XMLHttpRequest();
xhr.open('GET', '/api/data');
xhr.onload = function() {
    if (xhr.status === 200) {
        console.log(JSON.parse(xhr.responseText));
    }
};
xhr.send();
```
</details>

**Intermediate: Q1** - How do you handle different types of errors in fetch requests?

<details>
<summary>Answer</summary>

Check `response.ok` and handle network vs HTTP errors:

```javascript
async function handleFetch() {
    try {
        const response = await fetch('/api/data');
        
        // HTTP errors (404, 500, etc.)
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        if (error instanceof TypeError) {
            // Network error
            console.error('Network error:', error);
        } else {
            // HTTP error
            console.error('HTTP error:', error);
        }
    }
}
```
</details>

**Intermediate: Q2** - How do you send POST requests with JSON data using fetch?

<details>
<summary>Answer</summary>

Set method, headers, and stringify the body:

```javascript
const userData = { name: 'John', email: 'john@example.com' };

fetch('/api/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(userData)
})
.then(response => response.json())
.then(data => console.log('Success:', data));

// With async/await
async function createUser(userData) {
    const response = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData)
    });
    return response.json();
}
```
</details>

## CORS

**Beginner: Q1** - What is CORS and why does it exist?

<details>
<summary>Answer</summary>

CORS (Cross-Origin Resource Sharing) is a security mechanism that restricts web pages from accessing resources from different origins:

```javascript
// Same origin (allowed)
// From: https://example.com
fetch('https://example.com/api/data'); // ✓ Same origin

// Cross-origin (needs CORS)
// From: https://example.com  
fetch('https://api.other.com/data'); // ✗ Different origin

// CORS exists to prevent malicious websites from:
// - Reading sensitive data from other sites
// - Making unauthorized requests on user's behalf
```
</details>

**Beginner: Q2** - What is a CORS error and when do you encounter it?

<details>
<summary>Answer</summary>

CORS error occurs when trying to access a resource from a different origin without proper headers:

```javascript
// This will cause CORS error if server doesn't allow it
fetch('https://api.external.com/data')
    .then(response => response.json())
    .catch(error => {
        // Error: "Access to fetch at 'https://api.external.com/data' 
        // from origin 'https://mysite.com' has been blocked by CORS policy"
        console.error('CORS error:', error);
    });

// Common scenarios:
// - API calls to different domains
// - Different ports (localhost:3000 → localhost:8080)
// - Different protocols (http → https)
```
</details>

**Beginner: Q3** - How do you enable CORS on the server side?

<details>
<summary>Answer</summary>

Set appropriate CORS headers on the server:

```javascript
// Express.js server
app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*'); // or specific domain
    res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
    next();
});

// Or use cors middleware
const cors = require('cors');
app.use(cors({
    origin: 'https://mywebsite.com',
    credentials: true
}));
```
</details>

**Intermediate: Q1** - What are preflight requests and when do they occur?

<details>
<summary>Answer</summary>

Preflight requests are OPTIONS requests sent before complex requests to check permissions:

```javascript
// Simple request (no preflight)
fetch('/api/data', {
    method: 'GET',
    headers: { 'Content-Type': 'text/plain' }
});

// Complex request (triggers preflight)
fetch('https://api.example.com/data', {
    method: 'PUT', // Non-simple method
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token' // Custom header
    },
    body: JSON.stringify({data: 'value'})
});

// Browser sends OPTIONS request first:
// OPTIONS /api/data
// Access-Control-Request-Method: PUT
// Access-Control-Request-Headers: authorization,content-type
```
</details>

**Intermediate: Q2** - How do you handle credentials in CORS requests?

<details>
<summary>Answer</summary>

Use `credentials: 'include'` and proper server headers:

```javascript
// Client side - include cookies/credentials
fetch('https://api.example.com/data', {
    method: 'GET',
    credentials: 'include' // Include cookies
});

// Server side - allow credentials
res.header('Access-Control-Allow-Credentials', 'true');
res.header('Access-Control-Allow-Origin', 'https://specificdomain.com'); // Cannot use '*' with credentials

// XMLHttpRequest version
xhr.withCredentials = true;
```
</details>

## WebSockets

**Beginner: Q1** - What are WebSockets and how are they different from HTTP?

<details>
<summary>Answer</summary>

WebSockets provide persistent, bi-directional communication between client and server:

| Feature | HTTP | WebSockets |
|---------|------|------------|
| Connection | Request-response | Persistent |
| Direction | Unidirectional | Bi-directional |
| Overhead | High (headers each request) | Low (after handshake) |
| Use case | Traditional web | Real-time apps |

```javascript
// HTTP - request/response
fetch('/api/data').then(response => response.json());

// WebSocket - persistent connection
const ws = new WebSocket('ws://localhost:8080');
ws.send('Hello Server'); // Can send anytime
```
</details>

**Beginner: Q2** - How do you create a WebSocket connection?

<details>
<summary>Answer</summary>

Use the `WebSocket` constructor:

```javascript
// Create connection
const socket = new WebSocket('ws://localhost:8080');

// Connection opened
socket.addEventListener('open', (event) => {
    console.log('Connected to server');
    socket.send('Hello Server!');
});

// Receive messages
socket.addEventListener('message', (event) => {
    console.log('Message from server:', event.data);
});

// Connection closed
socket.addEventListener('close', (event) => {
    console.log('Connection closed');
});
```
</details>

**Beginner: Q3** - What events can you listen for on a WebSocket?

<details>
<summary>Answer</summary>

Four main events: `open`, `message`, `close`, and `error`:

```javascript
const socket = new WebSocket('ws://localhost:8080');

socket.onopen = () => console.log('Connection opened');
socket.onmessage = (event) => console.log('Received:', event.data);
socket.onclose = (event) => console.log('Connection closed:', event.code);
socket.onerror = (error) => console.error('WebSocket error:', error);

// Or with addEventListener
socket.addEventListener('open', handleOpen);
socket.addEventListener('message', handleMessage);
socket.addEventListener('close', handleClose);
socket.addEventListener('error', handleError);
```
</details>

**Intermediate: Q1** - How do you handle WebSocket reconnection and error recovery?

<details>
<summary>Answer</summary>

Implement automatic reconnection with exponential backoff:

```javascript
class ReconnectingWebSocket {
    constructor(url) {
        this.url = url;
        this.reconnectAttempts = 0;
        this.maxReconnectAttempts = 5;
        this.reconnectInterval = 1000;
        this.connect();
    }
    
    connect() {
        this.socket = new WebSocket(this.url);
        
        this.socket.onopen = () => {
            console.log('Connected');
            this.reconnectAttempts = 0;
        };
        
        this.socket.onclose = () => this.handleReconnect();
        this.socket.onerror = () => this.handleReconnect();
    }
    
    handleReconnect() {
        if (this.reconnectAttempts < this.maxReconnectAttempts) {
            setTimeout(() => {
                this.reconnectAttempts++;
                this.connect();
            }, this.reconnectInterval * Math.pow(2, this.reconnectAttempts));
        }
    }
}
```
</details>

**Intermediate: Q2** - When would you choose WebSockets over HTTP polling?

<details>
<summary>Answer</summary>

Choose WebSockets for real-time, bi-directional communication:

**Use WebSockets for:**
- Real-time chat applications
- Live data feeds (stock prices, sports scores)
- Online gaming
- Collaborative editing (Google Docs)
- Live notifications

**Use HTTP polling for:**
- Occasional data updates
- Simple request-response patterns
- Better caching requirements
- Simpler server infrastructure

```javascript
// HTTP Polling (every 5 seconds)
setInterval(() => {
    fetch('/api/notifications').then(res => res.json());
}, 5000);

// WebSocket (real-time)
socket.onmessage = (event) => {
    const notification = JSON.parse(event.data);
    displayNotification(notification);
};
```
</details>

## Service Workers

**Beginner: Q1** - What are Service Workers and what are they used for?

<details>
<summary>Answer</summary>

Service Workers are scripts that run in the background, separate from web pages, enabling offline functionality and caching:

**Main uses:**
- Offline functionality
- Background sync
- Push notifications
- Caching strategies
- Network request interception

```javascript
// Service Worker acts as a proxy between app and network
self.addEventListener('fetch', (event) => {
    event.respondWith(
        caches.match(event.request)
            .then(response => response || fetch(event.request))
    );
});
```
</details>

**Beginner: Q2** - How do you register a Service Worker?

<details>
<summary>Answer</summary>

Register in main thread using `navigator.serviceWorker.register()`:

```javascript
// In main app (main thread)
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js')
        .then(registration => {
            console.log('SW registered:', registration);
        })
        .catch(error => {
            console.log('SW registration failed:', error);
        });
}

// Check if already registered
navigator.serviceWorker.ready.then(registration => {
    console.log('SW is ready:', registration);
});
```
</details>

**Beginner: Q3** - What is the lifecycle of a Service Worker?

<details>
<summary>Answer</summary>

Service Worker has three main lifecycle events:

```javascript
// sw.js - Service Worker file

// 1. Install - SW is installing
self.addEventListener('install', (event) => {
    console.log('SW installing');
    event.waitUntil(
        caches.open('v1').then(cache => {
            return cache.addAll(['/index.html', '/app.js']);
        })
    );
    self.skipWaiting(); // Activate immediately
});

// 2. Activate - SW is activated
self.addEventListener('activate', (event) => {
    console.log('SW activated');
    event.waitUntil(clients.claim()); // Take control immediately
});

// 3. Fetch - Intercept network requests
self.addEventListener('fetch', (event) => {
    // Handle requests
});
```
</details>

**Intermediate: Q1** - How do you implement caching strategies with Service Workers?

<details>
<summary>Answer</summary>

Implement different caching strategies based on resource type:

```javascript
self.addEventListener('fetch', (event) => {
    const url = new URL(event.request.url);
    
    // Cache First (for static assets)
    if (url.pathname.includes('/static/')) {
        event.respondWith(
            caches.match(event.request)
                .then(response => response || fetch(event.request))
        );
    }
    
    // Network First (for API calls)
    else if (url.pathname.includes('/api/')) {
        event.respondWith(
            fetch(event.request)
                .catch(() => caches.match(event.request))
        );
    }
    
    // Stale While Revalidate (for pages)
    else {
        event.respondWith(
            caches.match(event.request)
                .then(response => {
                    const fetchPromise = fetch(event.request);
                    if (response) {
                        fetchPromise.then(res => {
                            caches.open('v1').then(cache => cache.put(event.request, res.clone()));
                        });
                        return response;
                    }
                    return fetchPromise;
                })
        );
    }
});
```
</details>

**Intermediate: Q2** - How do Service Workers enable offline functionality?

<details>
<summary>Answer</summary>

Service Workers cache resources and serve them when offline:

```javascript
// Cache essential resources during install
self.addEventListener('install', (event) => {
    event.waitUntil(
        caches.open('offline-v1').then(cache => {
            return cache.addAll([
                '/',
                '/index.html',
                '/app.js',
                '/styles.css',
                '/offline.html'
            ]);
        })
    );
});

// Serve cached content when offline
self.addEventListener('fetch', (event) => {
    event.respondWith(
        caches.match(event.request)
            .then(response => {
                // Return cached version or try network
                return response || fetch(event.request);
            })
            .catch(() => {
                // If both fail, show offline page
                if (event.request.destination === 'document') {
                    return caches.match('/offline.html');
                }
            })
    );
});
```
</details>

## try/catch/finally

**Beginner: Q1** - How do you handle errors using try/catch/finally?

<details>
<summary>Answer</summary>

Use try/catch/finally blocks to handle errors gracefully:

```javascript
try {
    // Code that might throw an error
    const data = JSON.parse(jsonString);
    console.log(data);
} catch (error) {
    // Handle the error
    console.error('Parsing failed:', error.message);
} finally {
    // Always executes (optional)
    console.log('Cleanup or logging');
}

// With functions
function safeOperation() {
    try {
        return riskyFunction();
    } catch (error) {
        return null; // Fallback value
    }
}
```
</details>

**Beginner: Q2** - When does the finally block execute?

<details>
<summary>Answer</summary>

The finally block always executes, regardless of success or error:

```javascript
function testFinally() {
    try {
        console.log('try'); // Executes
        return 'from try';
    } catch (error) {
        console.log('catch'); // Doesn't execute
        return 'from catch';
    } finally {
        console.log('finally'); // Always executes
    }
}

testFinally(); 
// Output: 'try', 'finally'
// Returns: 'from try'

// Finally executes even with errors
try {
    throw new Error('test');
} catch (e) {
    console.log('caught');
} finally {
    console.log('cleanup'); // Still executes
}
```
</details>

**Beginner: Q3** - Can you have try/catch without finally, or try/finally without catch?

<details>
<summary>Answer</summary>

Yes, both combinations are valid:

```javascript
// try/catch without finally
try {
    riskyOperation();
} catch (error) {
    handleError(error);
}

// try/finally without catch (error will propagate)
try {
    riskyOperation();
} finally {
    cleanup(); // Always runs, even if error occurs
}

// All three together
try {
    riskyOperation();
} catch (error) {
    handleError(error);
} finally {
    cleanup();
}
```
</details>

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

<details>
<summary>Answer</summary>

Output: `'finally'`

The finally block's return statement overrides the try block's return:

```javascript
function test() {
    try {
        return 'try';    // This is overridden
    } finally {
        return 'finally'; // This wins
    }
}
console.log(test()); // 'finally'

// Without return in finally
function test2() {
    try {
        return 'try';
    } finally {
        console.log('cleanup'); // No return
    }
}
console.log(test2()); // 'try'
```
</details>

**Intermediate: Q2** - How do you handle async errors in try/catch blocks?

<details>
<summary>Answer</summary>

Use try/catch with async/await for async operations:

```javascript
// Async/await with try/catch
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Fetch failed:', error);
        throw error; // Re-throw if needed
    } finally {
        console.log('Request completed');
    }
}

// Won't work with regular Promises
try {
    fetch('/api/data'); // Returns Promise, won't catch
} catch (error) {
    // This won't catch fetch errors
}

// Use .catch() instead
fetch('/api/data')
    .then(response => response.json())
    .catch(error => console.error(error));
```
</details>

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

<details>
<summary>Answer</summary>

The `test()` method checks if a pattern exists in a string and returns a boolean:

```javascript
const regex = /hello/i;
const text = 'Hello World';

// test() returns true or false
console.log(regex.test(text)); // true
console.log(regex.test('goodbye')); // false

// Common use cases
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const isValidEmail = emailRegex.test('user@example.com'); // true

const phoneRegex = /^\d{3}-\d{3}-\d{4}$/;
const isValidPhone = phoneRegex.test('123-456-7890'); // true
const isInvalidPhone = phoneRegex.test('123456789'); // false

// With global flag (be careful)
const globalRegex = /\d/g;
console.log(globalRegex.test('123')); // true (first call)
console.log(globalRegex.test('123')); // true (second call)
console.log(globalRegex.test('123')); // true (third call)
console.log(globalRegex.test('123')); // false (fourth call - lastIndex reset)

// Reset lastIndex for global regex
globalRegex.lastIndex = 0; // Reset to start over

// Practical validation example
function validateInput(input) {
    const patterns = {
        username: /^[a-zA-Z0-9_]{3,16}$/,
        password: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/,
        url: /^https?:\/\/.+/
    };
    
    return {
        username: patterns.username.test(input.username),
        password: patterns.password.test(input.password),
        url: patterns.url.test(input.url)
    };
}

// Usage
const result = validateInput({
    username: 'john_doe',
    password: 'MyPass123',
    url: 'https://example.com'
});
console.log(result); // { username: true, password: true, url: true }
```
</details>

**Beginner: Q2** - How do you use `match()` to find patterns in a string?

<details>
<summary>Answer</summary>

The `match()` method returns an array with match details or `null` if no match found:

```javascript
const text = 'The price is $25.99 and tax is $2.60';

// Simple match (first occurrence)
const simpleMatch = text.match(/\$\d+\.\d+/);
console.log(simpleMatch); // ['$25.99', index: 13, input: '...', groups: undefined]

// Global match (all occurrences)
const globalMatch = text.match(/\$\d+\.\d+/g);
console.log(globalMatch); // ['$25.99', '$2.60']

// With capture groups
const emailText = 'Contact us at support@example.com or sales@test.org';
const emailMatch = emailText.match(/(\w+)@(\w+\.\w+)/);
console.log(emailMatch);
// [
//   'support@example.com',  // Full match
//   'support',              // First capture group
//   'example.com',          // Second capture group
//   index: 14,
//   input: '...',
//   groups: undefined
// ]

// Global match with capture groups (use matchAll instead)
const allEmails = emailText.match(/(\w+)@(\w+\.\w+)/g);
console.log(allEmails); // ['support@example.com', 'sales@test.org'] - no groups

// matchAll() for global matches with capture groups
const allEmailsWithGroups = [...emailText.matchAll(/(\w+)@(\w+\.\w+)/g)];
console.log(allEmailsWithGroups);
// [
//   ['support@example.com', 'support', 'example.com', ...],
//   ['sales@test.org', 'sales', 'test.org', ...]
// ]

// Extracting data from strings
const logEntry = '2023-10-17 14:30:25 ERROR User login failed';
const logMatch = logEntry.match(/^(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) (\w+) (.+)$/);

if (logMatch) {
    const [, date, time, level, message] = logMatch;
    console.log({ date, time, level, message });
}

// No match returns null
const noMatch = 'hello world'.match(/\d+/);
console.log(noMatch); // null

// Safe extraction with optional chaining
const amount = text.match(/\$(\d+\.\d+)/)?.[1];
console.log(amount); // '25.99'
```
</details>

**Beginner: Q3** - How do you replace text using regular expressions?

<details>
<summary>Answer</summary>

Use `replace()` method with regex patterns for powerful text substitution:

```javascript
const text = 'Hello World, Hello Universe';

// Simple replacement (first occurrence)
const replaced = text.replace(/Hello/, 'Hi');
console.log(replaced); // 'Hi World, Hello Universe'

// Global replacement (all occurrences)
const globalReplaced = text.replace(/Hello/g, 'Hi');
console.log(globalReplaced); // 'Hi World, Hi Universe'

// Case-insensitive replacement
const caseInsensitive = 'HELLO world hello'.replace(/hello/gi, 'Hi');
console.log(caseInsensitive); // 'Hi world Hi'

// Using capture groups in replacement
const phoneNumber = 'Call me at 123-456-7890';
const formatted = phoneNumber.replace(/(\d{3})-(\d{3})-(\d{4})/, '($1) $2-$3');
console.log(formatted); // 'Call me at (123) 456-7890'

// Named capture groups (ES2018)
const date = '2023-10-17';
const namedGroups = date.replace(
    /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/,
    '$<month>/$<day>/$<year>'
);
console.log(namedGroups); // '10/17/2023'

// Function as replacement
const prices = 'Item costs $10.99 and shipping is $5.50';
const withTax = prices.replace(/\$(\d+\.\d+)/g, (match, price) => {
    const amount = parseFloat(price);
    const withTax = (amount * 1.08).toFixed(2);
    return `$${withTax}`;
});
console.log(withTax); // 'Item costs $11.87 and shipping is $5.94'

// Advanced replacement with function
const camelCase = 'hello-world-example'.replace(/-(\w)/g, (match, letter) => {
    return letter.toUpperCase();
});
console.log(camelCase); // 'helloWorldExample'

// Sanitizing user input
const userInput = '<script>alert("xss")</script>Hello<b>World</b>';
const sanitized = userInput.replace(/<[^>]*>/g, '');
console.log(sanitized); // 'alert("xss")HelloWorld'

// Masking sensitive data
const creditCard = 'My card number is 1234-5678-9012-3456';
const masked = creditCard.replace(/(\d{4}-\d{4}-\d{4}-)(\d{4})/, '$1****');
console.log(masked); // 'My card number is 1234-5678-9012-****'

// replaceAll() method (ES2021)
const text2 = 'cat and cat and cat';
const allReplaced = text2.replaceAll('cat', 'dog');
console.log(allReplaced); // 'dog and dog and dog'

// replaceAll with regex
const regexReplaceAll = text2.replaceAll(/cat/g, 'dog');
console.log(regexReplaceAll); // 'dog and dog and dog'
```
</details>

**Intermediate: Q1** - What's the difference between `match()` and `exec()` methods?

<details>
<summary>Answer</summary>

`match()` is a string method while `exec()` is a regex method with different behaviors:

```javascript
const text = 'The numbers are 123, 456, and 789';
const regex = /\d+/g;

// MATCH() - String method
// Simple usage
console.log(text.match(/\d+/)); // ['123', index: 16, input: '...']

// Global match - returns array of matches only
console.log(text.match(/\d+/g)); // ['123', '456', '789']

// With capture groups - global loses group details
console.log(text.match(/(\d)(\d)(\d)/g)); // ['123', '456', '789'] - no groups

// EXEC() - Regex method  
// Always returns detailed info for one match
const regex1 = /\d+/;
console.log(regex1.exec(text)); // ['123', index: 16, input: '...']

// Global exec() - stateful, remembers lastIndex
const regex2 = /\d+/g;
console.log(regex2.exec(text)); // ['123', index: 16, ...]
console.log(regex2.exec(text)); // ['456', index: 21, ...]
console.log(regex2.exec(text)); // ['789', index: 31, ...]
console.log(regex2.exec(text)); // null (reset lastIndex to 0)

// With capture groups - exec preserves groups even globally
const groupRegex = /(\d)(\d)(\d)/g;
let result;
while ((result = groupRegex.exec(text)) !== null) {
    console.log(`Full: ${result[0]}, Groups: ${result[1]}, ${result[2]}, ${result[3]}`);
    // Full: 123, Groups: 1, 2, 3
    // Full: 456, Groups: 4, 5, 6
    // Full: 789, Groups: 7, 8, 9
}

// Key differences:
// 1. Return behavior
const str = 'abc 123 def 456';
const pattern = /\d+/g;

// match() with global: returns all matches
console.log(str.match(pattern)); // ['123', '456']

// exec() with global: returns one match at a time
console.log(pattern.exec(str)); // ['123', ...]
console.log(pattern.exec(str)); // ['456', ...]

// 2. Capture groups
const emailText = 'contact@example.com and support@test.org';
const emailRegex = /(\w+)@(\w+\.\w+)/g;

// match() loses capture group details with global
console.log(emailText.match(emailRegex)); // ['contact@example.com', 'support@test.org']

// exec() preserves capture groups
emailRegex.lastIndex = 0; // Reset
console.log(emailRegex.exec(emailText)); // ['contact@example.com', 'contact', 'example.com', ...]

// 3. Method ownership
const regex3 = /test/;
const string = 'test string';

console.log(string.match(regex3)); // String method
console.log(regex3.exec(string));  // Regex method

// 4. Performance consideration
// For single matches: similar performance
// For multiple matches: exec() can be more memory efficient

// Practical example: Processing log entries
const logText = '2023-01-01 ERROR Failed\n2023-01-02 INFO Success';
const logRegex = /^(\d{4}-\d{2}-\d{2}) (\w+) (.+)$/gm;

// Using exec() to process each match
let match;
const logEntries = [];
while ((match = logRegex.exec(logText)) !== null) {
    logEntries.push({
        date: match[1],
        level: match[2], 
        message: match[3]
    });
}
console.log(logEntries);
```
</details>

**Intermediate: Q2** - How do you use capture groups in regex replacement?

<details>
<summary>Answer</summary>

Capture groups allow you to reference matched parts in replacement strings:

```javascript
// Basic capture groups with $1, $2, etc.
const fullName = 'John Doe';
const reversed = fullName.replace(/(\w+) (\w+)/, '$2, $1');
console.log(reversed); // 'Doe, John'

// Phone number formatting
const phone = '1234567890';
const formatted = phone.replace(/(\d{3})(\d{3})(\d{4})/, '($1) $2-$3');
console.log(formatted); // '(123) 456-7890'

// Date format conversion
const date = '2023-10-17';
const usFormat = date.replace(/(\d{4})-(\d{2})-(\d{2})/, '$2/$3/$1');
console.log(usFormat); // '10/17/2023'

// Named capture groups (ES2018)
const email = 'user@example.com';
const masked = email.replace(
    /(?<username>\w+)@(?<domain>\w+\.\w+)/, 
    '****@$<domain>'
);
console.log(masked); // '****@example.com'

// Using functions for complex replacements
const text = 'The items cost $10.50 and $25.99';
const withTax = text.replace(/\$(\d+\.\d+)/g, (match, price) => {
    const amount = parseFloat(price);
    const total = (amount * 1.08).toFixed(2);
    return `$${total} (inc. tax)`;
});
console.log(withTax); // 'The items cost $11.34 (inc. tax) and $28.07 (inc. tax)'

// Multiple capture groups in function
const logEntry = '2023-10-17 14:30:25 ERROR Database connection failed';
const formatted = logEntry.replace(
    /^(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) (\w+) (.+)$/,
    (match, date, time, level, message) => {
        const formattedLevel = level.padEnd(5);
        return `[${date} ${time}] ${formattedLevel}: ${message}`;
    }
);
console.log(formatted); // '[2023-10-17 14:30:25] ERROR: Database connection failed'

// Camel case conversion
const kebabCase = 'hello-world-example';
const camelCase = kebabCase.replace(/-(\w)/g, (match, letter) => {
    return letter.toUpperCase();
});
console.log(camelCase); // 'helloWorldExample'

// URL parameter extraction and replacement
const url = 'https://api.example.com/users/123/posts/456';
const newUrl = url.replace(
    /\/users\/(\d+)\/posts\/(\d+)/,
    '/api/v2/user/$1/post/$2'
);
console.log(newUrl); // 'https://api.example.com/api/v2/user/123/post/456'

// HTML tag replacement
const html = '<p>Hello</p><div>World</div>';
const modified = html.replace(/<(\w+)>([^<]+)<\/\1>/g, (match, tag, content) => {
    return `<${tag} class="styled">${content}</${tag}>`;
});
console.log(modified); // '<p class="styled">Hello</p><div class="styled">World</div>'

// Credit card masking with groups
const cardNumber = '1234-5678-9012-3456';
const masked = cardNumber.replace(
    /^(\d{4})-(\d{4})-(\d{4})-(\d{4})$/, 
    '$1-****-****-$4'
);
console.log(masked); // '1234-****-****-3456'

// Advanced: Conditional replacement
const text2 = 'Contact: john@example.com or call 123-456-7890';
const processed = text2.replace(
    /([\w.]+@[\w.]+)|(\d{3}-\d{3}-\d{4})/g,
    (match, email, phone) => {
        if (email) {
            return `<a href="mailto:${email}">${email}</a>`;
        } else if (phone) {
            return `<a href="tel:${phone}">${phone}</a>`;
        }
        return match;
    }
);
console.log(processed);
// 'Contact: <a href="mailto:john@example.com">john@example.com</a> or call <a href="tel:123-456-7890">123-456-7890</a>'

// Special replacement patterns
const specialText = 'Hello World';
console.log(specialText.replace(/(Hello)/, '$1 Beautiful')); // 'Hello Beautiful World'
console.log(specialText.replace(/(Hello)/, '$` - $1 - $\'')); // ' - Hello -  World World'
// $` = everything before match
// $& = the match itself  
// $' = everything after match
```
</details>

## Memory leaks

**Beginner: Q1** - What is a memory leak in JavaScript?

<details>
<summary>Answer</summary>

A memory leak occurs when memory is allocated but never freed, causing gradual memory consumption increase:

```javascript
// Memory leak example - global variables
function createLeak() {
    // Creates global variable (not garbage collected)
    leakedData = new Array(1000000).fill('data');
}
createLeak();

// Proper approach
function noLeak() {
    // Local variable (garbage collected when function ends)
    const localData = new Array(1000000).fill('data');
    return localData.length;
}

// Memory leak - forgotten timer
function startTimer() {
    const data = new Array(1000000).fill('data');
    
    setInterval(() => {
        // Timer keeps reference to 'data'
        console.log(data.length);
    }, 1000);
    // Timer never cleared - 'data' never freed
}

// Fixed version
function startTimerFixed() {
    const data = new Array(1000000).fill('data');
    
    const timerId = setInterval(() => {
        console.log(data.length);
    }, 1000);
    
    // Clear timer when done
    setTimeout(() => {
        clearInterval(timerId);
    }, 10000);
}

// Memory leak signs:
// 1. Gradually increasing memory usage
// 2. Slower performance over time
// 3. Browser/app crashes
// 4. High memory consumption in dev tools

// Common leak scenarios:
// - Forgotten event listeners
// - Unclosed timers/intervals
// - Circular references
// - Large objects in closures
// - Detached DOM nodes
```
</details>

**Beginner: Q2** - What are common causes of memory leaks?

<details>
<summary>Answer</summary>

Common memory leak causes include forgotten cleanup, circular references, and global variables:

```javascript
// 1. Global variables
function badPractice() {
    globalVar = new Array(1000000); // Accidental global
    window.anotherGlobal = 'data';   // Explicit global
}

// Fix: Use proper scoping
function goodPractice() {
    const localVar = new Array(1000000);
    // Automatically cleaned up
}

// 2. Forgotten timers
function leakyTimer() {
    const largeData = new Array(1000000);
    
    setInterval(() => {
        console.log('Running with large data');
    }, 1000);
    // Timer never cleared - keeps largeData in memory
}

// Fix: Clear timers
function cleanTimer() {
    const largeData = new Array(1000000);
    
    const timerId = setInterval(() => {
        console.log('Running');
    }, 1000);
    
    // Clear when component unmounts/page changes
    return () => clearInterval(timerId);
}

// 3. Event listeners not removed
function setupEventListeners() {
    const largeObject = { data: new Array(1000000) };
    
    function handleClick() {
        console.log(largeObject.data.length);
    }
    
    document.addEventListener('click', handleClick);
    // Listener keeps reference to largeObject
}

// Fix: Remove listeners
function setupEventListenersFixed() {
    const largeObject = { data: new Array(1000000) };
    
    function handleClick() {
        console.log(largeObject.data.length);
    }
    
    document.addEventListener('click', handleClick);
    
    // Cleanup function
    return () => document.removeEventListener('click', handleClick);
}

// 4. Circular references (old IE issue, mostly resolved)
function circularReference() {
    const obj1 = {};
    const obj2 = {};
    
    obj1.ref = obj2;
    obj2.ref = obj1; // Circular reference
    
    // Modern JS engines handle this, but can still cause issues
}

// 5. Closures holding large objects
function closureLeak() {
    const largeData = new Array(1000000).fill('data');
    
    return function() {
        // This closure keeps largeData alive
        console.log('Function called');
    };
}

// Fix: Don't capture unnecessary variables
function closureFixed() {
    const largeData = new Array(1000000).fill('data');
    const summary = largeData.length; // Extract what you need
    
    return function() {
        console.log(`Data length: ${summary}`);
        // Only keeps 'summary', not entire array
    };
}

// 6. Detached DOM nodes
function domLeak() {
    const parent = document.getElementById('parent');
    const child = document.createElement('div');
    parent.appendChild(child);
    
    // Remove parent but keep reference to child
    parent.remove();
    
    // 'child' is detached but still referenced - memory leak
    return child;
}

// Fix: Clear references
function domFixed() {
    const parent = document.getElementById('parent');
    const child = document.createElement('div');
    parent.appendChild(child);
    
    parent.remove();
    // Don't return/store references to detached nodes
}
```
</details>

**Beginner: Q3** - How can event listeners cause memory leaks?

<details>
<summary>Answer</summary>

Event listeners keep references to callback functions and their closures, preventing garbage collection:

```javascript
// Memory leak example
function setupPage() {
    const largeData = new Array(1000000).fill('user data');
    const button = document.getElementById('myButton');
    
    button.addEventListener('click', function() {
        // This callback keeps 'largeData' in memory
        console.log(`Processing ${largeData.length} items`);
    });
    
    // When page changes, button removed but listener still exists
    // 'largeData' never gets garbage collected
}

// Fix 1: Remove event listeners
function setupPageFixed() {
    const largeData = new Array(1000000).fill('user data');
    const button = document.getElementById('myButton');
    
    function handleClick() {
        console.log(`Processing ${largeData.length} items`);
    }
    
    button.addEventListener('click', handleClick);
    
    // Cleanup function
    return function cleanup() {
        button.removeEventListener('click', handleClick);
    };
}

// Fix 2: Use AbortController (modern approach)
function setupPageWithAbort() {
    const largeData = new Array(1000000).fill('user data');
    const button = document.getElementById('myButton');
    const controller = new AbortController();
    
    button.addEventListener('click', function() {
        console.log(`Processing ${largeData.length} items`);
    }, { signal: controller.signal });
    
    // Cleanup all listeners at once
    return () => controller.abort();
}

// Fix 3: Avoid capturing large objects
function setupPageOptimized() {
    const largeData = new Array(1000000).fill('user data');
    const button = document.getElementById('myButton');
    
    // Extract only what you need
    const dataLength = largeData.length;
    
    button.addEventListener('click', function() {
        // Only captures 'dataLength', not entire array
        console.log(`Processing ${dataLength} items`);
    });
}

// React/Vue component example (common leak)
class LeakyComponent {
    constructor() {
        this.largeData = new Array(1000000);
        this.handleResize = this.handleResize.bind(this);
    }
    
    mount() {
        // Adding listener but not removing
        window.addEventListener('resize', this.handleResize);
    }
    
    handleResize() {
        console.log('Resizing with data:', this.largeData.length);
    }
    
    // Missing cleanup method
}

// Fixed component
class FixedComponent {
    constructor() {
        this.largeData = new Array(1000000);
        this.handleResize = this.handleResize.bind(this);
    }
    
    mount() {
        window.addEventListener('resize', this.handleResize);
    }
    
    unmount() {
        // Proper cleanup
        window.removeEventListener('resize', this.handleResize);
        this.largeData = null;
    }
    
    handleResize() {
        if (this.largeData) {
            console.log('Resizing with data:', this.largeData.length);
        }
    }
}

// Modern framework patterns
// React hooks
function useEventListener(event, handler) {
    useEffect(() => {
        document.addEventListener(event, handler);
        
        return () => {
            document.removeEventListener(event, handler);
        };
    }, [event, handler]);
}

// Vue composition API
function useEventListener(target, event, handler) {
    onMounted(() => {
        target.addEventListener(event, handler);
    });
    
    onUnmounted(() => {
        target.removeEventListener(event, handler);
    });
}

// Best practices:
// 1. Always remove listeners in cleanup
// 2. Use AbortController for multiple listeners
// 3. Avoid capturing large objects in callbacks
// 4. Use weak references when possible
// 5. Framework lifecycle methods for cleanup
```
</details>

**Intermediate: Q1** - How do closures potentially cause memory leaks?

<details>
<summary>Answer</summary>

Closures can cause memory leaks by retaining references to variables longer than necessary:

```javascript
// Memory leak: Closure keeps entire scope alive
function createLeak() {
    const largeArray = new Array(1000000).fill('data');
    const anotherLargeArray = new Array(1000000).fill('more data');
    const smallValue = 42;
    
    return function() {
        // Only uses smallValue, but closure keeps ALL variables
        console.log(smallValue);
    };
    // largeArray and anotherLargeArray never freed
}

const leakyFunction = createLeak();

// Fix 1: Extract only needed values
function createFixed1() {
    const largeArray = new Array(1000000).fill('data');
    const anotherLargeArray = new Array(1000000).fill('more data');
    const smallValue = 42;
    
    // Process large arrays here, don't capture them
    const processedValue = largeArray.length + anotherLargeArray.length;
    
    return function() {
        // Only captures smallValue and processedValue
        console.log(smallValue, processedValue);
    };
}

// Fix 2: Null out large variables
function createFixed2() {
    let largeArray = new Array(1000000).fill('data');
    const smallValue = 42;
    
    // Process data while available
    const result = largeArray.reduce((sum, item) => sum + item.length, 0);
    
    // Clear reference
    largeArray = null;
    
    return function() {
        console.log(smallValue, result);
        // largeArray is null, can be garbage collected
    };
}

// Common leak: Timer callbacks with closures
function setupTimer() {
    const userData = fetchUserData(); // Large object
    const config = getConfig();       // Another large object
    
    setInterval(() => {
        // Only needs one property, but keeps entire objects
        console.log(userData.name);
    }, 1000);
}

// Fixed version
function setupTimerFixed() {
    const userData = fetchUserData();
    const config = getConfig();
    
    // Extract only needed values
    const userName = userData.name;
    
    const timerId = setInterval(() => {
        console.log(userName);
    }, 1000);
    
    // Clear timer and references
    setTimeout(() => {
        clearInterval(timerId);
    }, 60000);
}

// Event handler closures
function setupEventHandlers() {
    const cache = new Map(); // Large cache object
    const settings = loadSettings(); // Large settings
    
    document.getElementById('button').addEventListener('click', function() {
        // Closure captures entire scope
        const key = this.dataset.key;
        console.log(cache.get(key));
    });
    
    // When button is removed, cache and settings still in memory
}

// Fixed with proper scoping
function setupEventHandlersFixed() {
    const cache = new Map();
    const settings = loadSettings();
    
    // Create handler with minimal closure
    const handleClick = createClickHandler(cache);
    
    document.getElementById('button').addEventListener('click', handleClick);
    
    // Return cleanup function
    return () => {
        document.getElementById('button').removeEventListener('click', handleClick);
        cache.clear(); // Explicit cleanup
    };
}

function createClickHandler(cache) {
    return function(event) {
        const key = event.target.dataset.key;
        console.log(cache.get(key));
    };
}

// Module pattern leaks
const LeakyModule = (function() {
    const massiveData = new Array(1000000);
    const config = { /* large config */ };
    
    return {
        // This keeps massiveData alive even if not used
        getConfigValue: function(key) {
            return config[key];
        },
        
        // This method never uses massiveData but closure keeps it
        simpleMethod: function() {
            console.log('Simple operation');
        }
    };
})();

// Fixed module pattern
const FixedModule = (function() {
    const massiveData = new Array(1000000);
    const config = { /* large config */ };
    
    // Process massive data immediately and clear it
    const processedResult = massiveData.reduce((sum, item) => sum + 1, 0);
    // Clear the reference
    massiveData = null;
    
    return {
        getConfigValue: function(key) {
            return config[key];
        },
        
        getProcessedCount: function() {
            return processedResult; // Uses processed value, not original array
        }
    };
})();

// React component closure leak (common pattern)
function BadComponent() {
    const [data, setData] = useState(new Array(1000000));
    
    const handleClick = useCallback(() => {
        // Captures entire 'data' array in closure
        console.log('Button clicked');
    }, []); // Empty dependency array but still captures data
    
    return <button onClick={handleClick}>Click</button>;
}

// Fixed React component
function GoodComponent() {
    const [data, setData] = useState(new Array(1000000));
    
    const handleClick = useCallback(() => {
        console.log('Button clicked');
    }, []); // No reference to data
    
    // Or if you need data:
    const handleClickWithData = useCallback(() => {
        console.log('Data length:', data.length);
    }, [data.length]); // Only depend on length, not entire array
    
    return <button onClick={handleClick}>Click</button>;
}
```
</details>

**Intermediate: Q2** - How do you detect and debug memory leaks in a web application?

<details>
<summary>Answer</summary>

Use browser dev tools, monitoring techniques, and code analysis to identify memory leaks:

```javascript
// 1. Chrome DevTools Memory Tab
// Steps:
// - Open DevTools → Memory tab
// - Take heap snapshot before action
// - Perform suspected leaky operation
// - Take another snapshot
// - Compare snapshots for growing objects

// 2. Performance monitoring in code
class MemoryMonitor {
    constructor() {
        this.measurements = [];
    }
    
    measureMemory() {
        if (performance.memory) {
            const memory = {
                used: performance.memory.usedJSHeapSize,
                total: performance.memory.totalJSHeapSize,
                limit: performance.memory.jsHeapSizeLimit,
                timestamp: Date.now()
            };
            
            this.measurements.push(memory);
            console.log('Memory usage:', memory);
            
            return memory;
        }
    }
    
    checkForLeaks() {
        if (this.measurements.length < 2) return false;
        
        const recent = this.measurements.slice(-5);
        const growing = recent.every((measurement, index) => {
            return index === 0 || measurement.used > recent[index - 1].used;
        });
        
        if (growing) {
            console.warn('Potential memory leak detected!');
            return true;
        }
        return false;
    }
}

// Usage
const monitor = new MemoryMonitor();
setInterval(() => {
    monitor.measureMemory();
    monitor.checkForLeaks();
}, 5000);

// 3. Automated leak detection
function detectLeaks() {
    let baseline = 0;
    let measurements = 0;
    
    const interval = setInterval(() => {
        if (performance.memory) {
            const current = performance.memory.usedJSHeapSize;
            
            if (measurements === 0) {
                baseline = current;
            } else if (measurements > 5) {
                const increase = current - baseline;
                const percentIncrease = (increase / baseline) * 100;
                
                if (percentIncrease > 50) {
                    console.error(`Potential leak: ${percentIncrease.toFixed(1)}% increase`);
                    // Alert development team or log to monitoring service
                }
            }
            
            measurements++;
        }
    }, 10000);
    
    // Return cleanup function
    return () => clearInterval(interval);
}

// 4. Tracking object creation/destruction
class LeakTracker {
    constructor() {
        this.created = new Map();
        this.destroyed = new Set();
    }
    
    trackCreation(id, type, size) {
        this.created.set(id, { type, size, timestamp: Date.now() });
    }
    
    trackDestruction(id) {
        this.destroyed.add(id);
    }
    
    getLeaks() {
        const leaks = [];
        for (const [id, info] of this.created) {
            if (!this.destroyed.has(id)) {
                leaks.push({ id, ...info });
            }
        }
        return leaks;
    }
    
    report() {
        const leaks = this.getLeaks();
        if (leaks.length > 0) {
            console.warn('Potential leaks found:', leaks);
        }
        return leaks;
    }
}

// Usage in classes
class TrackedComponent {
    constructor(id) {
        this.id = id;
        window.leakTracker?.trackCreation(id, 'Component', 1000);
    }
    
    destroy() {
        window.leakTracker?.trackDestruction(this.id);
    }
}

// 5. WeakMap for tracking without leaks
const componentRegistry = new WeakMap();

function trackComponent(component, data) {
    // WeakMap doesn't prevent garbage collection
    componentRegistry.set(component, data);
}

// 6. Debugging specific leak patterns
function debugEventListeners() {
    const originalAddEventListener = EventTarget.prototype.addEventListener;
    const originalRemoveEventListener = EventTarget.prototype.removeEventListener;
    
    const listenerRegistry = new Map();
    
    EventTarget.prototype.addEventListener = function(type, listener, options) {
        const key = `${this.constructor.name}:${type}`;
        if (!listenerRegistry.has(key)) {
            listenerRegistry.set(key, []);
        }
        listenerRegistry.get(key).push(listener);
        
        console.log(`Added listener: ${key}, Total: ${listenerRegistry.get(key).length}`);
        return originalAddEventListener.call(this, type, listener, options);
    };
    
    EventTarget.prototype.removeEventListener = function(type, listener, options) {
        const key = `${this.constructor.name}:${type}`;
        if (listenerRegistry.has(key)) {
            const listeners = listenerRegistry.get(key);
            const index = listeners.indexOf(listener);
            if (index !== -1) {
                listeners.splice(index, 1);
                console.log(`Removed listener: ${key}, Remaining: ${listeners.length}`);
            }
        }
        return originalRemoveEventListener.call(this, type, listener, options);
    };
    
    // Check for orphaned listeners
    setInterval(() => {
        for (const [key, listeners] of listenerRegistry) {
            if (listeners.length > 10) {
                console.warn(`Potential leak: ${listeners.length} listeners for ${key}`);
            }
        }
    }, 30000);
}

// 7. Production monitoring
class ProductionMemoryMonitor {
    constructor() {
        this.alertThreshold = 100 * 1024 * 1024; // 100MB
        this.checkInterval = 60000; // 1 minute
        this.start();
    }
    
    start() {
        setInterval(() => {
            if (performance.memory && performance.memory.usedJSHeapSize > this.alertThreshold) {
                this.reportLeak();
            }
        }, this.checkInterval);
    }
    
    reportLeak() {
        const memoryInfo = {
            used: performance.memory.usedJSHeapSize,
            total: performance.memory.totalJSHeapSize,
            url: window.location.href,
            userAgent: navigator.userAgent,
            timestamp: new Date().toISOString()
        };
        
        // Send to monitoring service
        fetch('/api/memory-leak-report', {
            method: 'POST',
            body: JSON.stringify(memoryInfo),
            headers: { 'Content-Type': 'application/json' }
        }).catch(console.error);
    }
}

// Tools and techniques:
// - Chrome DevTools Memory tab
// - Performance.measureUserAgentSpecificMemory()
// - Heap snapshots comparison
// - Performance timeline
// - Third-party tools: Clinic.js, Memwatch
```
</details>

## Performance optimization techniques

**Beginner: Q1** - What are some basic techniques to optimize JavaScript performance?

<details>
<summary>Answer</summary>

Basic JavaScript performance optimization techniques:

```javascript
// 1. Minimize global variable access
// Slow
for (let i = 0; i < 1000; i++) {
    console.log(globalVariable.property);
}

// Fast - cache global references
const cached = globalVariable.property;
for (let i = 0; i < 1000; i++) {
    console.log(cached);
}

// 2. Use efficient data structures
// Slow - array search
const users = [/*large array*/];
const user = users.find(u => u.id === targetId);

// Fast - Map for lookups
const userMap = new Map(users.map(u => [u.id, u]));
const user = userMap.get(targetId);

// 3. Avoid unnecessary function calls
// Slow
const items = getItems();
for (let i = 0; i < items.length; i++) {
    if (isValid(items[i])) { // Function called every iteration
        process(items[i]);
    }
}

// Fast - cache function results
const items = getItems();
const validItems = items.filter(isValid); // Filter once
validItems.forEach(process);

// 4. Use const/let instead of var (block scoping)
// Slower
for (var i = 0; i < 1000; i++) {
    var temp = data[i] * 2;
    results.push(temp);
}

// Faster
for (let i = 0; i < 1000; i++) {
    const temp = data[i] * 2;
    results.push(temp);
}

// 5. Optimize object property access
// Slow - repeated property access
function processUser(user) {
    if (user.profile.settings.notifications.email) {
        sendEmail(user.profile.settings.notifications.email);
    }
}

// Fast - destructure once
function processUser(user) {
    const { email } = user.profile.settings.notifications;
    if (email) {
        sendEmail(email);
    }
}

// 6. Use array methods efficiently
const numbers = [1, 2, 3, 4, 5];

// Slow - multiple iterations
const doubled = numbers.map(n => n * 2);
const filtered = doubled.filter(n => n > 4);
const sum = filtered.reduce((a, b) => a + b, 0);

// Fast - single iteration
const result = numbers.reduce((acc, n) => {
    const doubled = n * 2;
    if (doubled > 4) {
        acc.sum += doubled;
        acc.items.push(doubled);
    }
    return acc;
}, { sum: 0, items: [] });

// 7. Debounce expensive operations
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

const expensiveSearch = debounce((query) => {
    // Expensive API call
    searchAPI(query);
}, 300);

// 8. Use requestAnimationFrame for animations
// Slow - setInterval
setInterval(() => {
    element.style.left = position + 'px';
    position += 1;
}, 16);

// Fast - requestAnimationFrame
function animate() {
    element.style.left = position + 'px';
    position += 1;
    requestAnimationFrame(animate);
}
animate();
```
</details>

**Beginner: Q2** - Why should you minimize DOM manipulation?

<details>
<summary>Answer</summary>

DOM manipulation is expensive because it triggers browser reflows and repaints:

```javascript
// SLOW - Multiple DOM manipulations
function addItems(items) {
    const container = document.getElementById('container');
    
    items.forEach(item => {
        const div = document.createElement('div');
        div.textContent = item.text;
        div.className = item.class;
        container.appendChild(div); // Causes reflow each time
    });
}

// FAST - Batch DOM updates
function addItemsFast(items) {
    const container = document.getElementById('container');
    const fragment = document.createDocumentFragment();
    
    items.forEach(item => {
        const div = document.createElement('div');
        div.textContent = item.text;
        div.className = item.class;
        fragment.appendChild(div); // No reflow
    });
    
    container.appendChild(fragment); // Single reflow
}

// SLOW - Reading layout properties repeatedly
function updateElements(elements) {
    elements.forEach(el => {
        el.style.left = el.offsetLeft + 10 + 'px'; // Forces layout calculation
        el.style.top = el.offsetTop + 10 + 'px';   // Forces layout again
    });
}

// FAST - Batch reads and writes
function updateElementsFast(elements) {
    // Read all values first
    const positions = elements.map(el => ({
        element: el,
        left: el.offsetLeft,
        top: el.offsetTop
    }));
    
    // Then write all changes
    positions.forEach(({ element, left, top }) => {
        element.style.left = left + 10 + 'px';
        element.style.top = top + 10 + 'px';
    });
}

// SLOW - Individual style updates
function styleElements(elements) {
    elements.forEach(el => {
        el.style.color = 'red';        // Repaint
        el.style.backgroundColor = 'blue'; // Repaint
        el.style.fontSize = '14px';    // Reflow + repaint
    });
}

// FAST - CSS classes or cssText
function styleElementsFast(elements) {
    elements.forEach(el => {
        el.className = 'optimized-style'; // Single repaint
        // Or use cssText
        // el.style.cssText = 'color: red; background: blue; font-size: 14px;';
    });
}

// SLOW - innerHTML in loops
function createList(items) {
    const container = document.getElementById('list');
    let html = '';
    
    items.forEach(item => {
        html += `<li>${item}</li>`;
        container.innerHTML = html; // Recreates DOM each time
    });
}

// FAST - Build string first, then set innerHTML once
function createListFast(items) {
    const container = document.getElementById('list');
    const html = items.map(item => `<li>${item}</li>`).join('');
    container.innerHTML = html; // Single DOM update
}

// Use virtual DOM concepts
class VirtualDOM {
    constructor() {
        this.pendingUpdates = [];
    }
    
    queueUpdate(element, property, value) {
        this.pendingUpdates.push({ element, property, value });
    }
    
    flush() {
        // Group updates by element
        const grouped = this.pendingUpdates.reduce((acc, update) => {
            if (!acc[update.element]) acc[update.element] = [];
            acc[update.element].push(update);
            return acc;
        }, {});
        
        // Apply all updates for each element at once
        Object.entries(grouped).forEach(([element, updates]) => {
            updates.forEach(({ property, value }) => {
                element.style[property] = value;
            });
        });
        
        this.pendingUpdates = [];
    }
}

// Minimize DOM queries
// SLOW - Query DOM repeatedly
function updateCards() {
    for (let i = 0; i < 100; i++) {
        const card = document.querySelector(`.card[data-id="${i}"]`);
        if (card) {
            card.textContent = `Updated ${i}`;
        }
    }
}

// FAST - Query once, cache results
function updateCardsFast() {
    const cards = document.querySelectorAll('.card');
    const cardMap = new Map();
    
    cards.forEach(card => {
        cardMap.set(card.dataset.id, card);
    });
    
    for (let i = 0; i < 100; i++) {
        const card = cardMap.get(String(i));
        if (card) {
            card.textContent = `Updated ${i}`;
        }
    }
}

// Performance impact:
// - DOM queries: 10-100x slower than JS operations
// - Layout (reflow): Forces recalculation of positions
// - Paint (repaint): Visual updates
// - Composite: Layer composition

// Best practices:
// 1. Batch DOM operations
// 2. Use DocumentFragment
// 3. Cache DOM references
// 4. Use CSS classes instead of inline styles
// 5. Minimize layout-triggering properties
// 6. Use transform/opacity for animations (GPU accelerated)
```
</details>

**Beginner: Q3** - How does code minification help with performance?

<details>
<summary>Answer</summary>

Code minification reduces file size by removing unnecessary characters and optimizing code:

```javascript
// Original code (before minification)
function calculateTotal(items, taxRate, discountPercent) {
    // Calculate subtotal
    let subtotal = 0;
    
    for (let i = 0; i < items.length; i++) {
        const item = items[i];
        subtotal += item.price * item.quantity;
    }
    
    // Apply discount
    const discountAmount = subtotal * (discountPercent / 100);
    const discountedTotal = subtotal - discountAmount;
    
    // Calculate tax
    const tax = discountedTotal * (taxRate / 100);
    
    // Return final total
    return discountedTotal + tax;
}

// Minified version (after minification)
function calculateTotal(t,a,e){let r=0;for(let n=0;n<t.length;n++){const l=t[n];r+=l.price*l.quantity}const n=r*(e/100),l=r-n,c=l*(a/100);return l+c}

// Benefits of minification:

// 1. Reduced file size (30-60% smaller)
// Original: 485 bytes
// Minified: 147 bytes (70% reduction)

// 2. Faster download times
const downloadTime = (fileSize, bandwidth) => {
    return fileSize / bandwidth;
};

console.log('Original:', downloadTime(485, 1000)); // 0.485 seconds
console.log('Minified:', downloadTime(147, 1000)); // 0.147 seconds

// 3. Less bandwidth usage
const monthlySavings = (originalSize, minifiedSize, requests) => {
    const savings = (originalSize - minifiedSize) * requests;
    return savings / 1024 / 1024; // MB saved
};

console.log('Monthly savings:', monthlySavings(485, 147, 100000)); // ~32 MB

// 4. Improved parsing time
// Less code to parse = faster JavaScript engine startup

// What minification removes/changes:
// - Whitespace (spaces, tabs, newlines)
// - Comments
// - Variable name shortening
// - Function name shortening (when safe)
// - Unnecessary semicolons
// - Dead code elimination

// Example transformations:
// Before minification:
const userData = {
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com'
};

function getUserFullName() {
    return userData.firstName + ' ' + userData.lastName;
}

// After minification:
const a={firstName:"John",lastName:"Doe",email:"john@example.com"};function getUserFullName(){return a.firstName+" "+a.lastName}

// Advanced optimizations in modern minifiers:
// 1. Dead code elimination
if (false) {
    console.log('This code is removed');
}

// 2. Constant folding
const result = 5 * 10; // Becomes: const result = 50;

// 3. Function inlining (for small functions)
function add(a, b) {
    return a + b;
}
const sum = add(5, 3); // Might become: const sum = 5 + 3;

// Tools for minification:
// - Terser (most popular)
// - UglifyJS
// - Google Closure Compiler
// - Webpack (built-in optimization)
// - Rollup
// - esbuild (fastest)

// Webpack configuration example:
module.exports = {
    mode: 'production', // Enables minification
    optimization: {
        minimize: true,
        minimizer: [
            new TerserPlugin({
                terserOptions: {
                    compress: {
                        drop_console: true, // Remove console.log
                        drop_debugger: true, // Remove debugger statements
                    },
                },
            }),
        ],
    },
};

// Performance impact:
// - 30-60% smaller files
// - Faster downloads (especially mobile)
// - Reduced parsing time
// - Lower bandwidth costs
// - Better Core Web Vitals scores

// Considerations:
// - Source maps for debugging
// - Gzip compression works well with minified code
// - HTTP/2 reduces benefit but still valuable
// - CDN caching amplifies benefits
```
</details>

**Intermediate: Q1** - What is lazy loading and how can it improve performance?

<details>
<summary>Answer</summary>

Lazy loading defers loading resources until they're actually needed:

```javascript
// 1. Image lazy loading
class LazyImageLoader {
    constructor() {
        this.imageObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    this.loadImage(entry.target);
                }
            });
        });
    }
    
    observe(img) {
        this.imageObserver.observe(img);
    }
    
    loadImage(img) {
        img.src = img.dataset.src;
        img.classList.add('loaded');
        this.imageObserver.unobserve(img);
    }
}

// HTML: <img data-src="actual-image.jpg" class="lazy-image" />

// 2. Module lazy loading (Dynamic imports)
async function loadFeature(featureName) {
    try {
        switch (featureName) {
            case 'charts':
                const { ChartLibrary } = await import('./chartLibrary.js');
                return new ChartLibrary();
                
            case 'editor':
                const { CodeEditor } = await import('./codeEditor.js');
                return new CodeEditor();
                
            default:
                throw new Error(`Unknown feature: ${featureName}`);
        }
    } catch (error) {
        console.error('Failed to load feature:', error);
    }
}

// Usage
document.getElementById('chart-btn').addEventListener('click', async () => {
    const chart = await loadFeature('charts');
    chart.render('#chart-container');
});

// 3. Component lazy loading (React example)
const LazyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
    return (
        <div>
            <Suspense fallback={<div>Loading...</div>}>
                <LazyComponent />
            </Suspense>
        </div>
    );
}

// 4. Data lazy loading (Infinite scroll)
class InfiniteScroll {
    constructor(container, loadMoreCallback) {
        this.container = container;
        this.loadMore = loadMoreCallback;
        this.loading = false;
        this.page = 1;
        
        this.setupObserver();
    }
    
    setupObserver() {
        const sentinel = document.createElement('div');
        sentinel.className = 'scroll-sentinel';
        this.container.appendChild(sentinel);
        
        const observer = new IntersectionObserver((entries) => {
            if (entries[0].isIntersecting && !this.loading) {
                this.loadNextPage();
            }
        });
        
        observer.observe(sentinel);
    }
    
    async loadNextPage() {
        this.loading = true;
        
        try {
            const data = await this.loadMore(this.page);
            this.renderItems(data);
            this.page++;
        } catch (error) {
            console.error('Failed to load more items:', error);
        } finally {
            this.loading = false;
        }
    }
    
    renderItems(items) {
        const fragment = document.createDocumentFragment();
        items.forEach(item => {
            const element = this.createItemElement(item);
            fragment.appendChild(element);
        });
        this.container.appendChild(fragment);
    }
}

// 5. Route-based code splitting
const routes = [
    {
        path: '/dashboard',
        component: () => import('./pages/Dashboard.js')
    },
    {
        path: '/profile',
        component: () => import('./pages/Profile.js')
    },
    {
        path: '/settings',
        component: () => import('./pages/Settings.js')
    }
];

async function loadRoute(path) {
    const route = routes.find(r => r.path === path);
    if (route) {
        const module = await route.component();
        return module.default;
    }
}

// 6. API data lazy loading
class DataManager {
    constructor() {
        this.cache = new Map();
        this.loading = new Set();
    }
    
    async getData(id) {
        // Return cached data immediately
        if (this.cache.has(id)) {
            return this.cache.get(id);
        }
        
        // Prevent duplicate requests
        if (this.loading.has(id)) {
            return this.waitForLoad(id);
        }
        
        this.loading.add(id);
        
        try {
            const data = await fetch(`/api/data/${id}`).then(r => r.json());
            this.cache.set(id, data);
            return data;
        } finally {
            this.loading.delete(id);
        }
    }
    
    waitForLoad(id) {
        return new Promise(resolve => {
            const check = () => {
                if (this.cache.has(id)) {
                    resolve(this.cache.get(id));
                } else {
                    setTimeout(check, 50);
                }
            };
            check();
        });
    }
}

// 7. Virtual scrolling for large lists
class VirtualList {
    constructor(container, items, itemHeight) {
        this.container = container;
        this.items = items;
        this.itemHeight = itemHeight;
        this.visibleCount = Math.ceil(container.clientHeight / itemHeight) + 2;
        this.startIndex = 0;
        
        this.setupScrolling();
        this.render();
    }
    
    setupScrolling() {
        this.container.addEventListener('scroll', () => {
            const newStartIndex = Math.floor(this.container.scrollTop / this.itemHeight);
            if (newStartIndex !== this.startIndex) {
                this.startIndex = newStartIndex;
                this.render();
            }
        });
    }
    
    render() {
        const endIndex = Math.min(this.startIndex + this.visibleCount, this.items.length);
        const visibleItems = this.items.slice(this.startIndex, endIndex);
        
        this.container.innerHTML = '';
        
        // Add spacer for items above viewport
        if (this.startIndex > 0) {
            const spacer = document.createElement('div');
            spacer.style.height = `${this.startIndex * this.itemHeight}px`;
            this.container.appendChild(spacer);
        }
        
        // Render visible items
        visibleItems.forEach((item, index) => {
            const element = this.createItemElement(item, this.startIndex + index);
            this.container.appendChild(element);
        });
        
        // Add spacer for items below viewport
        const remainingItems = this.items.length - endIndex;
        if (remainingItems > 0) {
            const spacer = document.createElement('div');
            spacer.style.height = `${remainingItems * this.itemHeight}px`;
            this.container.appendChild(spacer);
        }
    }
}

// Performance benefits:
// - Faster initial page load
// - Reduced memory usage
// - Better Core Web Vitals
// - Improved user experience
// - Lower bandwidth usage
// - Faster time to interactive
```
</details>

**Intermediate: Q2** - How do you optimize loops and iterations for better performance?

<details>
<summary>Answer</summary>

Optimize loops by reducing work inside iterations and choosing efficient patterns:

```javascript
// 1. Cache array length
// SLOW - length calculated every iteration
const items = new Array(10000).fill().map((_, i) => i);
for (let i = 0; i < items.length; i++) {
    console.log(items[i]);
}

// FAST - cache length
for (let i = 0, len = items.length; i < len; i++) {
    console.log(items[i]);
}

// 2. Reverse loops (sometimes faster)
// FAST - counting down to 0
for (let i = items.length - 1; i >= 0; i--) {
    console.log(items[i]);
}

// 3. Use appropriate loop type
const data = new Array(100000).fill().map((_, i) => ({ id: i, value: Math.random() }));

// Benchmark different approaches
console.time('for loop');
for (let i = 0; i < data.length; i++) {
    if (data[i].value > 0.5) data[i].processed = true;
}
console.timeEnd('for loop');

console.time('for...of');
for (const item of data) {
    if (item.value > 0.5) item.processed = true;
}
console.timeEnd('for...of');

console.time('forEach');
data.forEach(item => {
    if (item.value > 0.5) item.processed = true;
});
console.timeEnd('forEach');

// 4. Avoid function calls inside loops
// SLOW - function called every iteration
function isEven(n) {
    return n % 2 === 0;
}

const numbers = Array.from({length: 100000}, (_, i) => i);
const evenNumbers = [];

for (const num of numbers) {
    if (isEven(num)) { // Function call overhead
        evenNumbers.push(num);
    }
}

// FAST - inline logic
const evenNumbersFast = [];
for (const num of numbers) {
    if (num % 2 === 0) { // No function call
        evenNumbersFast.push(num);
    }
}

// 5. Break early when possible
// SLOW - continues even after finding result
function findUser(users, targetId) {
    let found = null;
    for (const user of users) {
        if (user.id === targetId) {
            found = user;
        }
    }
    return found;
}

// FAST - early return
function findUserFast(users, targetId) {
    for (const user of users) {
        if (user.id === targetId) {
            return user; // Exit immediately
        }
    }
    return null;
}

// 6. Minimize DOM operations in loops
// SLOW - DOM manipulation every iteration
const container = document.getElementById('container');
for (let i = 0; i < 1000; i++) {
    const div = document.createElement('div');
    div.textContent = `Item ${i}`;
    container.appendChild(div); // Reflow every time
}

// FAST - batch DOM operations
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
    const div = document.createElement('div');
    div.textContent = `Item ${i}`;
    fragment.appendChild(div);
}
container.appendChild(fragment); // Single reflow

// 7. Use efficient data structures
// SLOW - array includes for lookups
const validIds = [1, 5, 10, 15, 20];
const items = Array.from({length: 10000}, (_, i) => ({ id: i }));

const filtered = items.filter(item => validIds.includes(item.id)); // O(n*m)

// FAST - Set for lookups
const validIdSet = new Set(validIds);
const filteredFast = items.filter(item => validIdSet.has(item.id)); // O(n)

// 8. Avoid creating objects/arrays in loops
// SLOW - creates new objects every iteration
const results = [];
for (let i = 0; i < 10000; i++) {
    results.push({
        index: i,
        value: Math.random(),
        metadata: { created: new Date() } // New object every iteration
    });
}

// FAST - reuse objects when possible
const results2 = [];
const baseMetadata = { created: new Date() };
for (let i = 0; i < 10000; i++) {
    results2.push({
        index: i,
        value: Math.random(),
        metadata: baseMetadata // Shared object
    });
}

// 9. Use array methods efficiently
const largeArray = new Array(100000).fill().map(() => Math.random());

// SLOW - multiple iterations
const processed = largeArray
    .map(x => x * 2)
    .filter(x => x > 1)
    .map(x => Math.floor(x));

// FAST - single iteration with reduce
const processedFast = largeArray.reduce((acc, x) => {
    const doubled = x * 2;
    if (doubled > 1) {
        acc.push(Math.floor(doubled));
    }
    return acc;
}, []);

// 10. Loop unrolling for critical paths
// Normal loop
function sumArray(arr) {
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        sum += arr[i];
    }
    return sum;
}

// Unrolled loop (process multiple items per iteration)
function sumArrayUnrolled(arr) {
    let sum = 0;
    let i = 0;
    
    // Process 4 items at once
    for (; i < arr.length - 3; i += 4) {
        sum += arr[i] + arr[i + 1] + arr[i + 2] + arr[i + 3];
    }
    
    // Handle remaining items
    for (; i < arr.length; i++) {
        sum += arr[i];
    }
    
    return sum;
}

// 11. Use TypedArrays for numeric operations
// SLOW - regular array
const regularArray = new Array(100000).fill().map(() => Math.random());
let sum1 = 0;
for (let i = 0; i < regularArray.length; i++) {
    sum1 += regularArray[i];
}

// FAST - TypedArray
const typedArray = new Float64Array(100000);
for (let i = 0; i < typedArray.length; i++) {
    typedArray[i] = Math.random();
}

let sum2 = 0;
for (let i = 0; i < typedArray.length; i++) {
    sum2 += typedArray[i]; // Faster numeric operations
}

// 12. Benchmark and profile
function benchmarkLoop(name, fn, iterations = 1000) {
    console.time(name);
    for (let i = 0; i < iterations; i++) {
        fn();
    }
    console.timeEnd(name);
}

// Test different approaches
benchmarkLoop('forEach', () => data.forEach(item => item.value *= 2));
benchmarkLoop('for loop', () => {
    for (let i = 0; i < data.length; i++) {
        data[i].value *= 2;
    }
});
benchmarkLoop('for...of', () => {
    for (const item of data) {
        item.value *= 2;
    }
});

// Performance considerations:
// - V8 engine optimizations
// - JIT compilation patterns
// - Memory allocation patterns
// - Cache locality
// - Branch prediction
```
</details>
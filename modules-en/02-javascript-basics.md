# Module 2: JavaScript Basics

## Module Objectives

This module provides a solid foundation in JavaScript - the world's most popular programming language. You will learn from basic concepts to important features for building web applications.

---

## 1. Introduction to JavaScript

### 1.1 What is JavaScript?

#### Concept

**JavaScript** is a programming language created by Brendan Eich in 1995. Originally designed to add interactivity to web pages, today JavaScript has become a versatile programming language:

- **Frontend**: React, Vue, Angular
- **Backend**: Node.js, Deno, Bun
- **Mobile**: React Native, Ionic
- **Desktop**: Electron
- **Game Development**: Phaser, Three.js

#### Characteristics

```javascript
// 1. Dynamic Typing - Flexible data types
let value = 42; // number
value = "Hello"; // string - OK!
value = true; // boolean - OK!

// 2. Interpreted - No compilation needed
// JavaScript code runs directly in browser or Node.js

// 3. Event-driven
document.addEventListener("click", function () {
  console.log("Clicked!");
});

// 4. Single-threaded with Event Loop
// Efficiently handles async operations
```

### 1.2 Running JavaScript

```javascript
// 1. In Browser - Console (F12 → Console)
console.log("Hello from browser!");

// 2. In HTML file
/*
<script>
  console.log("Hello!");
</script>

// Or external file
<script src="app.js"></script>
*/

// 3. With Node.js
// Terminal: node app.js

// 4. Online playgrounds
// - CodePen, JSFiddle, CodeSandbox
```

---

## 2. Variables

### 2.1 Variable Declaration

#### Concept

Variables are "containers" to store data. JavaScript has 3 ways to declare variables:

```javascript
// var - old way (ES5), function scoped
var oldWay = "Avoid using var";

// let - ES6+, block scoped, can be reassigned
let counter = 0;
counter = 1; // ✅ OK

// const - ES6+, block scoped, cannot be reassigned
const PI = 3.14159;
// PI = 3.14; // ❌ Error: Assignment to constant variable
```

### 2.2 Naming Conventions

```javascript
// ✅ Camel Case - JavaScript standard
let firstName = "John";
let getUserById = function() {};

// ✅ UPPER_SNAKE_CASE for constants
const MAX_SIZE = 100;
const API_URL = "https://api.example.com";

// ✅ PascalCase for Classes
class UserAccount {}

// ❌ Avoid
let first_name = "John";  // snake_case
let FIRSTNAME = "John";   // ALL CAPS for regular variables
let 1stName = "John";     // Starting with number

// Naming rules:
// - Start with letter, _, or $
// - Don't use reserved words (let, const, function, etc.)
// - Case-sensitive: name ≠ Name ≠ NAME
```

### 2.3 var vs let vs const

```javascript
// 1. Different scope
function scopeDemo() {
  if (true) {
    var varVariable = "var"; // function scoped
    let letVariable = "let"; // block scoped
    const constVariable = "const"; // block scoped
  }

  console.log(varVariable); // ✅ "var"
  // console.log(letVariable);  // ❌ ReferenceError
  // console.log(constVariable); // ❌ ReferenceError
}

// 2. Different hoisting
console.log(x); // undefined (hoisted)
var x = 5;

// console.log(y); // ReferenceError (TDZ)
let y = 5;

// 3. Re-declaration
var a = 1;
var a = 2; // ✅ OK

let b = 1;
// let b = 2; // ❌ SyntaxError

// 4. const with Objects/Arrays
const user = { name: "John" };
user.name = "Jane"; // ✅ OK - mutating
// user = {};        // ❌ Error - reassigning

const arr = [1, 2, 3];
arr.push(4); // ✅ OK - [1, 2, 3, 4]
// arr = [5, 6];     // ❌ Error

// Best Practice:
// - Default to const
// - Use let when you need to reassign
// - Avoid var
```

---

## 3. Data Types

### 3.1 Primitive Types

```javascript
// 1. Number - Both integer and floating-point
let age = 25;
let price = 19.99;
let negative = -10;
let infinity = Infinity;
let notANumber = NaN; // Not a Number

// Special values
console.log(1 / 0); // Infinity
console.log(-1 / 0); // -Infinity
console.log("abc" / 2); // NaN

// Number methods
console.log(Number.isInteger(25)); // true
console.log(Number.isNaN(NaN)); // true
console.log(Number.parseFloat("3.14")); // 3.14

// 2. String - Character sequence
let singleQuotes = "Hello";
let doubleQuotes = "World";
let backticks = `Hello ${singleQuotes}`; // Template literal

// String methods
let str = "JavaScript";
console.log(str.length); // 10
console.log(str.toUpperCase()); // "JAVASCRIPT"
console.log(str.toLowerCase()); // "javascript"
console.log(str.indexOf("Script")); // 4
console.log(str.slice(0, 4)); // "Java"
console.log(str.split("")); // ["J","a","v","a",...]
console.log(str.includes("Script")); // true
console.log(str.startsWith("Java")); // true
console.log(str.replace("Java", "Type")); // "TypeScript"

// 3. Boolean - true/false
let isActive = true;
let isCompleted = false;

// Truthy and Falsy values
// Falsy: false, 0, "", null, undefined, NaN
// Truthy: everything else

// 4. undefined - Variable not yet assigned
let notDefined;
console.log(notDefined); // undefined

// 5. null - Intentional "nothing" value
let empty = null;

// 6. Symbol (ES6) - Unique value
let sym1 = Symbol("id");
let sym2 = Symbol("id");
console.log(sym1 === sym2); // false

// 7. BigInt (ES2020) - Large integers
let bigNumber = 9007199254740991n;
let anotherBig = BigInt("9007199254740991");
```

### 3.2 Reference Types

```javascript
// 1. Object - Collection of key-value pairs
let user = {
  name: "John",
  age: 30,
  isAdmin: true,
  address: {
    city: "New York",
    country: "USA",
  },
};

// Accessing properties
console.log(user.name); // "John" - dot notation
console.log(user["age"]); // 30 - bracket notation
console.log(user.address.city); // "New York"

// Adding/modifying/deleting properties
user.email = "john@example.com"; // add
user.age = 31; // modify
delete user.isAdmin; // delete

// 2. Array - Ordered list
let fruits = ["Apple", "Banana", "Orange"];
let mixed = [1, "two", true, null, { name: "John" }];

// Accessing elements
console.log(fruits[0]); // "Apple"
console.log(fruits.length); // 3

// Array methods (will learn more later)
fruits.push("Mango"); // add to end
fruits.pop(); // remove from end
fruits.unshift("Grape"); // add to beginning
fruits.shift(); // remove from beginning

// 3. Function - Also an object
function greet(name) {
  return `Hello, ${name}!`;
}

// 4. Date
let now = new Date();
let birthday = new Date("1990-05-15");
console.log(now.getFullYear()); // 2024
console.log(now.getMonth()); // 0-11
console.log(now.getDate()); // 1-31
```

### 3.3 Type Checking

```javascript
// typeof operator
console.log(typeof 42); // "number"
console.log(typeof "hello"); // "string"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" ⚠️ Bug!
console.log(typeof {}); // "object"
console.log(typeof []); // "object"
console.log(typeof function () {}); // "function"
console.log(typeof Symbol()); // "symbol"

// Check for Array
console.log(Array.isArray([])); // true
console.log(Array.isArray({})); // false

// Check for null
const value = null;
console.log(value === null); // true

// instanceof for objects
console.log([] instanceof Array); // true
console.log({} instanceof Object); // true
console.log(new Date() instanceof Date); // true
```

---

## 4. Operators

### 4.1 Arithmetic Operators

```javascript
// Basic
let a = 10,
  b = 3;
console.log(a + b); // 13 - Addition
console.log(a - b); // 7  - Subtraction
console.log(a * b); // 30 - Multiplication
console.log(a / b); // 3.333... - Division
console.log(a % b); // 1  - Modulo (remainder)
console.log(a ** b); // 1000 - Exponentiation (ES7)

// Increment/Decrement
let x = 5;
console.log(x++); // 5 (post-increment, returns then increments)
console.log(x); // 6
console.log(++x); // 7 (pre-increment, increments then returns)
console.log(x--); // 7
console.log(--x); // 5

// String concatenation
console.log("Hello" + " " + "World"); // "Hello World"
console.log("Price: " + 100); // "Price: 100"
```

### 4.2 Comparison Operators

```javascript
// Comparison
console.log(5 > 3); // true
console.log(5 < 3); // false
console.log(5 >= 5); // true
console.log(5 <= 4); // false

// Equality
console.log(5 == "5"); // true  (loose equality - with type coercion)
console.log(5 === "5"); // false (strict equality - no coercion)
console.log(5 != "5"); // false
console.log(5 !== "5"); // true

// ⚠️ Always use === and !== to avoid bugs
console.log(0 == false); // true
console.log(0 === false); // false
console.log(null == undefined); // true
console.log(null === undefined); // false
```

### 4.3 Logical Operators

```javascript
// AND (&&) - true if ALL are true
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false

// OR (||) - true if AT LEAST ONE is true
console.log(true || false); // true
console.log(false || false); // false

// NOT (!) - Inverts
console.log(!true); // false
console.log(!false); // true
console.log(!!0); // false (double negation → boolean)
console.log(!!"hello"); // true

// Short-circuit evaluation
let name = null;
console.log(name || "Anonymous"); // "Anonymous"

let user = { name: "John" };
console.log(user && user.name); // "John"

// Nullish coalescing (??) - ES2020
let value = null;
console.log(value ?? "default"); // "default"
console.log(0 ?? "default"); // 0 (0 is not null/undefined)
console.log(0 || "default"); // "default" (0 is falsy)
```

### 4.4 Assignment Operators

```javascript
let x = 10;

x += 5; // x = x + 5  → 15
x -= 3; // x = x - 3  → 12
x *= 2; // x = x * 2  → 24
x /= 4; // x = x / 4  → 6
x %= 4; // x = x % 4  → 2
x **= 3; // x = x ** 3 → 8

// Logical assignment (ES2021)
let a = null;
a ||= "default"; // a = a || "default" → "default"
a &&= "updated"; // a = a && "updated" → "updated"
a ??= "fallback"; // a = a ?? "fallback" → "updated"
```

### 4.5 Ternary Operator

```javascript
// condition ? valueIfTrue : valueIfFalse
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";
console.log(status); // "Adult"

// Nested ternary (should limit use)
let score = 85;
let grade = score >= 90 ? "A" : score >= 80 ? "B" : score >= 70 ? "C" : "F";
console.log(grade); // "B"
```

---

## 5. Control Flow

### 5.1 if...else

```javascript
// Basic if
let temperature = 25;

if (temperature > 30) {
  console.log("It's hot!");
}

// if...else
if (temperature > 30) {
  console.log("It's hot!");
} else {
  console.log("It's not hot.");
}

// if...else if...else
if (temperature > 30) {
  console.log("It's hot!");
} else if (temperature > 20) {
  console.log("It's warm.");
} else if (temperature > 10) {
  console.log("It's cool.");
} else {
  console.log("It's cold!");
}

// Shorthand for simple conditions
let isRaining = true;
if (isRaining) console.log("Take an umbrella!");

// With logical operators
let age = 25;
let hasLicense = true;
if (age >= 18 && hasLicense) {
  console.log("Can drive");
}
```

### 5.2 switch

```javascript
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
    console.log("Midweek");
    break;
  case "Friday":
    console.log("TGIF!");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend!");
    break;
  default:
    console.log("Invalid day");
}

// ⚠️ Don't forget break, otherwise it will "fall through"
let fruit = "apple";
switch (fruit) {
  case "apple":
    console.log("Apple");
  // no break → continues to next case
  case "banana":
    console.log("Banana");
    break;
  default:
    console.log("Other");
}
// Output: "Apple" and "Banana"
```

### 5.3 Loops

#### for loop

```javascript
// Basic for loop
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// Iterate over array
let fruits = ["Apple", "Banana", "Orange"];
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// Nested loops
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(`${i} x ${j} = ${i * j}`);
  }
}
```

#### for...of (ES6)

```javascript
// Iterate over iterable values
let colors = ["red", "green", "blue"];

for (let color of colors) {
  console.log(color); // "red", "green", "blue"
}

// With strings
for (let char of "Hello") {
  console.log(char); // "H", "e", "l", "l", "o"
}

// Get index too
for (let [index, color] of colors.entries()) {
  console.log(`${index}: ${color}`);
}
```

#### for...in

```javascript
// Iterate over object keys
let user = { name: "John", age: 30, city: "New York" };

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}
// "name: John", "age: 30", "city: New York"

// ⚠️ Avoid using for...in with arrays
let arr = ["a", "b", "c"];
for (let index in arr) {
  console.log(index); // "0", "1", "2" (strings, not numbers!)
}
```

#### while & do...while

```javascript
// while - Checks condition first
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

// do...while - Runs at least once
let num = 10;
do {
  console.log(num); // 10 (runs once even though condition is false)
  num++;
} while (num < 5);
```

### 5.4 break & continue

```javascript
// break - Exit the loop
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0, 1, 2, 3, 4
}

// continue - Skip current iteration
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0, 1, 3, 4
}

// Label - For nested loops
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer;
    console.log(`${i}, ${j}`);
  }
}
```

---

## 6. Functions

### 6.1 Function Declaration

```javascript
// Basic declaration
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("John")); // "Hello, John!"

// Hoisting - function declarations are hoisted
sayHi(); // ✅ Works!
function sayHi() {
  console.log("Hi!");
}
```

### 6.2 Function Expression

```javascript
// Assign function to variable
const greet = function (name) {
  return `Hello, ${name}!`;
};

// Not hoisted
// sayBye(); // ❌ Error
const sayBye = function () {
  console.log("Bye!");
};
```

### 6.3 Arrow Functions (ES6)

```javascript
// Basic syntax
const add = (a, b) => {
  return a + b;
};

// Shorthand - 1 expression
const addShort = (a, b) => a + b;

// 1 parameter - no parentheses needed
const double = (x) => x * 2;

// No parameters
const sayHello = () => "Hello!";

// Return object literal - needs parentheses
const createUser = (name, age) => ({ name, age });

// ⚠️ Arrow functions don't have:
// - Their own this (inherit from parent scope)
// - arguments object
// - Cannot be used as constructor
```

### 6.4 Parameters & Arguments

```javascript
// Default parameters (ES6)
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}
console.log(greet()); // "Hello, Guest!"
console.log(greet("John")); // "Hello, John!"
console.log(greet("John", "Hi")); // "Hi, John!"

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

// arguments object (don't use with arrow functions)
function oldSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

// Destructuring parameters
function createUser({ name, age, city = "Unknown" }) {
  return `${name}, ${age}, ${city}`;
}
createUser({ name: "John", age: 30 });
```

### 6.5 Return Values

```javascript
// Single return
function add(a, b) {
  return a + b;
}

// Multiple returns (early return pattern)
function divide(a, b) {
  if (b === 0) {
    return null; // Early return
  }
  return a / b;
}

// Return multiple values (via object or array)
function getMinMax(arr) {
  return {
    min: Math.min(...arr),
    max: Math.max(...arr),
  };
}
const { min, max } = getMinMax([1, 5, 3, 9, 2]);

// No return → returns undefined
function noReturn() {
  console.log("No return");
}
console.log(noReturn()); // undefined
```

### 6.6 Callback Functions

```javascript
// Function passed as argument
function processArray(arr, callback) {
  const result = [];
  for (let item of arr) {
    result.push(callback(item));
  }
  return result;
}

const numbers = [1, 2, 3, 4];
const doubled = processArray(numbers, (x) => x * 2);
console.log(doubled); // [2, 4, 6, 8]

// Common use cases
// 1. Event handlers
button.addEventListener("click", function () {
  console.log("Clicked!");
});

// 2. Array methods
const filtered = numbers.filter((n) => n > 2);

// 3. Async operations
setTimeout(() => {
  console.log("After 1 second");
}, 1000);
```

### 6.7 IIFE (Immediately Invoked Function Expression)

```javascript
// Function called immediately when defined
(function () {
  console.log("IIFE executed!");
})();

// With arrow function
(() => {
  console.log("Arrow IIFE");
})();

// With parameters
(function (name) {
  console.log(`Hello, ${name}!`);
})("John");

// Use case: Create private scope
const counter = (function () {
  let count = 0; // private

  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
  };
})();

console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
```

---

## 7. Arrays

### 7.1 Creating and Accessing Arrays

```javascript
// Create array
const fruits = ["Apple", "Banana", "Orange"];
const numbers = new Array(1, 2, 3); // Less common
const empty = [];

// Access elements
console.log(fruits[0]); // "Apple"
console.log(fruits[2]); // "Orange"
console.log(fruits[-1]); // undefined (negative not supported)
console.log(fruits.at(-1)); // "Orange" (ES2022)

// Modify element
fruits[1] = "Mango";
console.log(fruits); // ["Apple", "Mango", "Orange"]

// Length
console.log(fruits.length); // 3
```

### 7.2 Basic Array Methods

```javascript
let arr = [1, 2, 3];

// Add/remove from end
arr.push(4); // [1, 2, 3, 4] - returns new length
arr.pop(); // [1, 2, 3] - returns removed element

// Add/remove from beginning
arr.unshift(0); // [0, 1, 2, 3]
arr.shift(); // [1, 2, 3]

// Splice - add/remove at any position
arr.splice(1, 1); // Remove 1 element from index 1 → [1, 3]
arr.splice(1, 0, 2); // Add 2 at index 1 → [1, 2, 3]
arr.splice(1, 1, "two"); // Replace → [1, "two", 3]

// Slice - cut array (doesn't modify original)
let sliced = [1, 2, 3, 4, 5].slice(1, 4); // [2, 3, 4]

// Concat - join arrays
let combined = [1, 2].concat([3, 4]); // [1, 2, 3, 4]

// Join - convert to string
console.log(["a", "b", "c"].join("-")); // "a-b-c"

// Includes - check if contains
console.log([1, 2, 3].includes(2)); // true

// IndexOf / LastIndexOf
console.log([1, 2, 3, 2].indexOf(2)); // 1
console.log([1, 2, 3, 2].lastIndexOf(2)); // 3

// Reverse & Sort (modifies original array!)
[1, 3, 2].reverse(); // [2, 3, 1]
[3, 1, 2].sort(); // [1, 2, 3] - sort as strings by default!
[3, 1, 2].sort((a, b) => a - b); // [1, 2, 3] - numeric sort
```

### 7.3 Array Iteration Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - Execute function for each element
numbers.forEach((num, index) => {
  console.log(`${index}: ${num}`);
});

// map - Transform each element, return new array
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - Filter elements, return new array
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]

// find - Find first matching element
const found = numbers.find((n) => n > 3);
console.log(found); // 4

// findIndex - Find index of first match
const foundIndex = numbers.findIndex((n) => n > 3);
console.log(foundIndex); // 3

// some - Check if ANY element matches
const hasEven = numbers.some((n) => n % 2 === 0);
console.log(hasEven); // true

// every - Check if ALL elements match
const allPositive = numbers.every((n) => n > 0);
console.log(allPositive); // true

// reduce - Reduce to single value
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15

// Chaining methods
const result = numbers
  .filter((n) => n % 2 === 0) // [2, 4]
  .map((n) => n * 2) // [4, 8]
  .reduce((a, b) => a + b, 0); // 12
```

### 7.4 Spread & Destructuring

```javascript
// Spread operator
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]
const copy = [...arr1]; // Shallow copy

// Destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// Skip elements
const [a, , c] = [1, 2, 3];
console.log(a, c); // 1, 3

// Default values
const [x = 0, y = 0] = [1];
console.log(x, y); // 1, 0

// Swap values
let p = 1,
  q = 2;
[p, q] = [q, p];
console.log(p, q); // 2, 1
```

---

## 8. Objects

### 8.1 Object Basics

```javascript
// Object literal
const user = {
  firstName: "John",
  lastName: "Doe",
  age: 30,
  isAdmin: false,

  // Method
  getFullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },

  // Shorthand method (ES6)
  greet() {
    return `Hello, I'm ${this.firstName}`;
  },
};

// Accessing properties
console.log(user.firstName); // "John"
console.log(user["lastName"]); // "Doe"
console.log(user.getFullName()); // "John Doe"

// Dynamic key
const key = "age";
console.log(user[key]); // 30
```

### 8.2 Object Operations

```javascript
let user = { name: "John", age: 30 };

// Add property
user.email = "john@example.com";
user["phone"] = "123456";

// Delete property
delete user.phone;

// Check if property exists
console.log("name" in user); // true
console.log(user.hasOwnProperty("name")); // true
console.log(user.address !== undefined); // false

// Object.keys, Object.values, Object.entries
console.log(Object.keys(user)); // ["name", "age", "email"]
console.log(Object.values(user)); // ["John", 30, "john@example.com"]
console.log(Object.entries(user)); // [["name", "John"], ["age", 30], ...]

// Iterate over object
for (let [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

### 8.3 Object Methods

```javascript
// Object.assign - Copy/merge objects
const target = { a: 1 };
const source = { b: 2 };
const merged = Object.assign(target, source);
console.log(merged); // { a: 1, b: 2 }

// Spread operator (ES6) - preferred way
const mergedSpread = { ...target, ...source, c: 3 };

// Object.freeze - Prevent modifications
const frozen = Object.freeze({ name: "John" });
frozen.name = "Jane"; // Silent fail
console.log(frozen.name); // "John"

// Object.seal - Prevent add/remove but allow modify
const sealed = Object.seal({ name: "John" });
sealed.name = "Jane"; // OK
sealed.age = 30; // Silent fail

// Computed property names (ES6)
const prop = "name";
const obj = {
  [prop]: "John",
  [`get${prop.charAt(0).toUpperCase() + prop.slice(1)}`]() {
    return this[prop];
  },
};
console.log(obj.name); // "John"
console.log(obj.getName()); // "John"
```

### 8.4 Destructuring Objects

```javascript
const user = {
  name: "John",
  age: 30,
  address: {
    city: "New York",
    country: "USA",
  },
};

// Basic destructuring
const { name, age } = user;
console.log(name, age); // "John", 30

// Rename variables
const { name: userName, age: userAge } = user;
console.log(userName); // "John"

// Default values
const { phone = "N/A" } = user;
console.log(phone); // "N/A"

// Nested destructuring
const {
  address: { city, country },
} = user;
console.log(city); // "New York"

// Rest pattern
const { name: n, ...rest } = user;
console.log(rest); // { age: 30, address: {...} }

// Function parameters
function greet({ name, age = 0 }) {
  console.log(`${name} is ${age}`);
}
greet(user);
```

### 8.5 this Keyword Basics

```javascript
// 'this' refers to the object that calls the method
const user = {
  name: "John",
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  },
};

user.greet(); // "Hello, I'm John"

// ⚠️ 'this' context can be lost
const greetFn = user.greet;
greetFn(); // "Hello, I'm undefined" (this = window/undefined)

// Fix with bind
const boundGreet = user.greet.bind(user);
boundGreet(); // "Hello, I'm John"

// Arrow functions don't have own 'this'
const user2 = {
  name: "Jane",
  greet: () => {
    console.log(`Hello, I'm ${this.name}`); // 'this' is outer scope
  },
};
user2.greet(); // "Hello, I'm undefined"
```

---

## 9. Error Handling

### 9.1 try...catch...finally

```javascript
// Basic try-catch
try {
  let result = someUndefinedFunction();
} catch (error) {
  console.log("An error occurred:", error.message);
}

// finally - always runs
try {
  // risky code
  throw new Error("Something went wrong");
} catch (error) {
  console.log("Caught:", error.message);
} finally {
  console.log("This always runs");
}

// Error object properties
try {
  throw new Error("Custom error");
} catch (error) {
  console.log(error.name); // "Error"
  console.log(error.message); // "Custom error"
  console.log(error.stack); // Stack trace
}
```

### 9.2 Throwing Errors

```javascript
// Throw built-in errors
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}

// Different error types
throw new TypeError("Expected a string");
throw new RangeError("Value out of range");
throw new ReferenceError("Variable not defined");
throw new SyntaxError("Invalid syntax");

// Custom error class
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

function validateAge(age) {
  if (age < 0 || age > 150) {
    throw new ValidationError("Invalid age");
  }
  return true;
}

try {
  validateAge(-5);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log("Validation failed:", error.message);
  } else {
    throw error; // Re-throw unexpected errors
  }
}
```

### 9.3 Error Handling Patterns

```javascript
// Guard clauses - Early returns
function processUser(user) {
  if (!user) {
    throw new Error("User is required");
  }
  if (!user.name) {
    throw new Error("User name is required");
  }
  // Process user...
}

// Optional chaining (?.) to prevent errors
const user = null;
console.log(user?.name); // undefined (no error)
console.log(user?.address?.city); // undefined

// Nullish coalescing for defaults
const name = user?.name ?? "Anonymous";

// Try-catch wrapper
function tryCatch(fn) {
  try {
    return [null, fn()];
  } catch (error) {
    return [error, null];
  }
}

const [error, result] = tryCatch(() => JSON.parse("invalid"));
if (error) {
  console.log("Failed to parse");
}
```

---

## 10. DOM Manipulation

### 10.1 Selecting Elements

```javascript
// Single element
const byId = document.getElementById("myId");
const byQuery = document.querySelector(".myClass");
const byQueryFirst = document.querySelector("div.container");

// Multiple elements
const byClass = document.getElementsByClassName("myClass");
const byTag = document.getElementsByTagName("div");
const byQueryAll = document.querySelectorAll(".myClass");

// Traversing DOM
const parent = element.parentElement;
const children = element.children;
const firstChild = element.firstElementChild;
const lastChild = element.lastElementChild;
const nextSibling = element.nextElementSibling;
const prevSibling = element.previousElementSibling;
```

### 10.2 Modifying Elements

```javascript
const element = document.querySelector("#myElement");

// Text content
element.textContent = "New text";
element.innerText = "Visible text only";
element.innerHTML = "<strong>HTML content</strong>";

// Attributes
element.setAttribute("data-id", "123");
element.getAttribute("data-id");
element.removeAttribute("data-id");
element.id = "newId";
element.className = "class1 class2";

// Classes
element.classList.add("active");
element.classList.remove("active");
element.classList.toggle("active");
element.classList.contains("active"); // true/false
element.classList.replace("old", "new");

// Styles
element.style.color = "red";
element.style.backgroundColor = "blue";
element.style.cssText = "color: red; padding: 10px;";
```

### 10.3 Creating & Removing Elements

```javascript
// Create element
const newDiv = document.createElement("div");
newDiv.textContent = "I'm new!";
newDiv.className = "new-element";

// Add to DOM
document.body.appendChild(newDiv);
parent.insertBefore(newDiv, referenceNode);
parent.append(element1, element2); // Multiple elements
parent.prepend(element); // Add to beginning

// Modern methods (ES6+)
referenceElement.before(newElement);
referenceElement.after(newElement);
referenceElement.replaceWith(newElement);

// Remove element
element.remove();
parent.removeChild(element);

// Clone element
const clone = element.cloneNode(true); // true = deep clone
```

### 10.4 Event Handling

```javascript
const button = document.querySelector("button");

// addEventListener (recommended)
button.addEventListener("click", function (event) {
  console.log("Clicked!", event);
});

// Arrow function
button.addEventListener("click", (e) => {
  console.log("Clicked!");
});

// Remove event listener
function handleClick(e) {
  console.log("Clicked!");
}
button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);

// Common events
element.addEventListener("click", handler);
element.addEventListener("dblclick", handler);
element.addEventListener("mouseenter", handler);
element.addEventListener("mouseleave", handler);
element.addEventListener("keydown", handler);
element.addEventListener("keyup", handler);
element.addEventListener("submit", handler);
element.addEventListener("change", handler);
element.addEventListener("input", handler);
element.addEventListener("load", handler);
element.addEventListener("scroll", handler);

// Event object
button.addEventListener("click", function (e) {
  e.target; // Element that triggered event
  e.currentTarget; // Element with listener
  e.type; // Event type
  e.preventDefault(); // Prevent default behavior
  e.stopPropagation(); // Stop bubbling
});

// Event delegation
document.querySelector("ul").addEventListener("click", function (e) {
  if (e.target.tagName === "LI") {
    console.log("Li clicked:", e.target.textContent);
  }
});
```

---

## 11. Asynchronous JavaScript Basics

### 11.1 setTimeout & setInterval

```javascript
// setTimeout - Run once after delay
setTimeout(() => {
  console.log("After 2 seconds");
}, 2000);

// Clear timeout
const timeoutId = setTimeout(() => {
  console.log("This won't run");
}, 5000);
clearTimeout(timeoutId);

// setInterval - Run repeatedly
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log(`Count: ${count}`);

  if (count >= 5) {
    clearInterval(intervalId);
  }
}, 1000);
```

### 11.2 Callbacks

```javascript
// Callback pattern
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "John" };
    callback(data);
  }, 1000);
}

fetchData((data) => {
  console.log("Received:", data);
});

// Callback with error handling
function fetchUser(id, onSuccess, onError) {
  setTimeout(() => {
    if (id > 0) {
      onSuccess({ id, name: "John" });
    } else {
      onError(new Error("Invalid ID"));
    }
  }, 1000);
}

fetchUser(
  1,
  (user) => console.log("User:", user),
  (error) => console.log("Error:", error.message)
);
```

### 11.3 Promises Basics

```javascript
// Creating a Promise
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("Data loaded!");
    } else {
      reject(new Error("Failed to load"));
    }
  }, 1000);
});

// Using a Promise
myPromise
  .then((result) => {
    console.log(result); // "Data loaded!"
    return "Next step";
  })
  .then((result) => {
    console.log(result); // "Next step"
  })
  .catch((error) => {
    console.log("Error:", error.message);
  })
  .finally(() => {
    console.log("Done!");
  });

// Promise.all - Wait for all
Promise.all([Promise.resolve(1), Promise.resolve(2), Promise.resolve(3)]).then(
  ([a, b, c]) => {
    console.log(a, b, c); // 1, 2, 3
  }
);

// Promise.race - First to settle
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve("Fast"), 100)),
  new Promise((resolve) => setTimeout(() => resolve("Slow"), 500)),
]).then((result) => {
  console.log(result); // "Fast"
});
```

### 11.4 Async/Await Basics

```javascript
// Async function
async function fetchData() {
  return "Data";
}
fetchData().then((data) => console.log(data)); // "Data"

// Await - Wait for Promise
async function fetchUser() {
  try {
    const response = await fetch("/api/user");
    const user = await response.json();
    return user;
  } catch (error) {
    console.log("Error:", error);
  }
}

// Sequential vs Parallel
async function sequential() {
  const a = await fetchA(); // Wait
  const b = await fetchB(); // Then wait
  return [a, b];
}

async function parallel() {
  const [a, b] = await Promise.all([fetchA(), fetchB()]);
  return [a, b];
}
```

---

## 12. Practice Exercises

### Exercise 1: Variables & Data Types

```javascript
// 1.1: Declare variables with different data types
// - Create variable for name (string), age (number), married (boolean)
// - Create object containing product info (name, price, inStock)
// - Create array containing 5 fruits

// 1.2: Type conversion
// - Convert string "123" to number
// - Convert number 456 to string
// - Check type of the above variables
```

### Exercise 2: Control Flow

```javascript
// 2.1: Write function to check even/odd number
function isEven(n) {
  // Implement
}

// 2.2: Write function to get grade from score
function getGrade(score) {
  // A: 90-100, B: 80-89, C: 70-79, D: 60-69, F: < 60
}

// 2.3: FizzBuzz - Print numbers from 1-100
// - If divisible by 3: "Fizz"
// - If divisible by 5: "Buzz"
// - If divisible by both 3 and 5: "FizzBuzz"
// - Otherwise: print the number
```

### Exercise 3: Functions

```javascript
// 3.1: Write function to calculate factorial
function factorial(n) {
  // Implement (both recursion and loop)
}

// 3.2: Write function to find max value in array
function findMax(arr) {
  // Implement
}

// 3.3: Write counter module with closure
function createCounter() {
  // Return object with increment, decrement, getCount, reset
}
```

### Exercise 4: Arrays

```javascript
// 4.1: Given number array, return array containing only even numbers, each multiplied by 2
// Input: [1, 2, 3, 4, 5]
// Output: [4, 8]

// 4.2: Calculate total age of all users
const users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 },
  { name: "Bob", age: 35 },
];

// 4.3: Remove duplicates from array
// Input: [1, 2, 2, 3, 3, 3]
// Output: [1, 2, 3]
```

### Exercise 5: Objects

```javascript
// 5.1: Deep clone object
function deepClone(obj) {
  // Implement (without using JSON.parse/stringify)
}

// 5.2: Merge multiple objects
function mergeObjects(...objects) {
  // Implement
}

// 5.3: Create Student class with methods
class Student {
  // Properties: name, grades (array)
  // Methods: addGrade, getAverage, getStatus
}
```

### Exercise 6: DOM & Events

```javascript
// 6.1: Create simple Todo List
// - Input to add task
// - Button to add task
// - List to display tasks
// - Click task to mark complete (strikethrough)
// - Button to delete task

// 6.2: Create Counter with UI
// - Display showing number
// - Buttons: increment, decrement, reset
// - Don't allow negative numbers
```

---

## 13. References

- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [W3Schools JavaScript Tutorial](https://www.w3schools.com/js/)
- [freeCodeCamp JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

---

## 14. Summary

### Key Points to Remember:

1. **Variables:**

   - Use `const` by default, `let` when you need to reassign
   - Avoid using `var`

2. **Data Types:**

   - 7 primitive types: string, number, boolean, null, undefined, symbol, bigint
   - Reference types: object, array, function
   - Use `===` instead of `==`

3. **Functions:**

   - Function declarations are hoisted
   - Arrow functions don't have their own `this`
   - Use default parameters and rest parameters

4. **Arrays:**

   - `map`, `filter`, `reduce` are the 3 most important methods
   - Spread operator to copy and merge arrays
   - Destructuring to extract values

5. **Objects:**

   - Dot notation vs bracket notation
   - Object methods: keys, values, entries
   - Destructuring to extract properties

6. **Error Handling:**

   - Always use try-catch for risky operations
   - Optional chaining (`?.`) to prevent errors

7. **Async:**
   - Callbacks → Promises → Async/Await
   - Prefer async/await for readable code

**Advice:** Practice, practice, practice! JavaScript basics are the foundation for all JavaScript frameworks and libraries.

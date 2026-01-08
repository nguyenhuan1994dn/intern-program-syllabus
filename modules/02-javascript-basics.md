# Module 2: JavaScript Basics

## Mục tiêu module

Module này cung cấp nền tảng vững chắc về JavaScript - ngôn ngữ lập trình phổ biến nhất trên thế giới. Bạn sẽ học từ các khái niệm cơ bản đến những tính năng quan trọng để xây dựng ứng dụng web.

---

## 1. Giới thiệu JavaScript

### 1.1 JavaScript là gì?

#### Khái niệm

**JavaScript** là ngôn ngữ lập trình được tạo ra bởi Brendan Eich vào năm 1995. Ban đầu được thiết kế để thêm tính tương tác cho web pages, ngày nay JavaScript đã trở thành ngôn ngữ lập trình đa năng:

- **Frontend**: React, Vue, Angular
- **Backend**: Node.js, Deno, Bun
- **Mobile**: React Native, Ionic
- **Desktop**: Electron
- **Game Development**: Phaser, Three.js

#### Đặc điểm

```javascript
// 1. Dynamic Typing - Kiểu dữ liệu linh hoạt
let value = 42; // number
value = "Hello"; // string - OK!
value = true; // boolean - OK!

// 2. Interpreted - Không cần compile
// Code JavaScript chạy trực tiếp trong browser hoặc Node.js

// 3. Event-driven - Hướng sự kiện
document.addEventListener("click", function () {
  console.log("Clicked!");
});

// 4. Single-threaded với Event Loop
// Xử lý async operations hiệu quả
```

### 1.2 Cách chạy JavaScript

```javascript
// 1. Trong Browser - Console (F12 → Console)
console.log("Hello from browser!");

// 2. Trong HTML file
/*
<script>
  console.log("Hello!");
</script>

// Hoặc external file
<script src="app.js"></script>
*/

// 3. Với Node.js
// Terminal: node app.js

// 4. Online playgrounds
// - CodePen, JSFiddle, CodeSandbox
```

---

## 2. Variables (Biến)

### 2.1 Khai báo biến

#### Khái niệm

Biến là "container" để lưu trữ dữ liệu. JavaScript có 3 cách khai báo biến:

```javascript
// var - cách cũ (ES5), function scoped
var oldWay = "Avoid using var";

// let - ES6+, block scoped, có thể reassign
let counter = 0;
counter = 1; // ✅ OK

// const - ES6+, block scoped, không thể reassign
const PI = 3.14159;
// PI = 3.14; // ❌ Error: Assignment to constant variable
```

### 2.2 Naming Conventions

```javascript
// ✅ Camel Case - Chuẩn JavaScript
let firstName = "John";
let getUserById = function() {};

// ✅ UPPER_SNAKE_CASE cho constants
const MAX_SIZE = 100;
const API_URL = "https://api.example.com";

// ✅ PascalCase cho Classes
class UserAccount {}

// ❌ Tránh
let first_name = "John";  // snake_case
let FIRSTNAME = "John";   // ALL CAPS cho biến thường
let 1stName = "John";     // Bắt đầu bằng số

// Quy tắc đặt tên:
// - Bắt đầu bằng chữ cái, _, hoặc $
// - Không dùng reserved words (let, const, function, etc.)
// - Case-sensitive: name ≠ Name ≠ NAME
```

### 2.3 var vs let vs const

```javascript
// 1. Scope khác nhau
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

// 2. Hoisting khác nhau
console.log(x); // undefined (hoisted)
var x = 5;

// console.log(y); // ReferenceError (TDZ)
let y = 5;

// 3. Re-declaration
var a = 1;
var a = 2; // ✅ OK

let b = 1;
// let b = 2; // ❌ SyntaxError

// 4. const với Objects/Arrays
const user = { name: "John" };
user.name = "Jane"; // ✅ OK - mutating
// user = {};        // ❌ Error - reassigning

const arr = [1, 2, 3];
arr.push(4); // ✅ OK - [1, 2, 3, 4]
// arr = [5, 6];     // ❌ Error

// Best Practice:
// - Mặc định dùng const
// - Dùng let khi cần reassign
// - Tránh var
```

---

## 3. Data Types (Kiểu dữ liệu)

### 3.1 Primitive Types

```javascript
// 1. Number - Số (cả integer và floating-point)
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

// 2. String - Chuỗi ký tự
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

// Truthy và Falsy values
// Falsy: false, 0, "", null, undefined, NaN
// Truthy: mọi thứ khác

// 4. undefined - Biến chưa được gán giá trị
let notDefined;
console.log(notDefined); // undefined

// 5. null - Giá trị "không có gì" một cách có chủ đích
let empty = null;

// 6. Symbol (ES6) - Giá trị unique
let sym1 = Symbol("id");
let sym2 = Symbol("id");
console.log(sym1 === sym2); // false

// 7. BigInt (ES2020) - Số nguyên lớn
let bigNumber = 9007199254740991n;
let anotherBig = BigInt("9007199254740991");
```

### 3.2 Reference Types

```javascript
// 1. Object - Tập hợp key-value pairs
let user = {
  name: "John",
  age: 30,
  isAdmin: true,
  address: {
    city: "Hanoi",
    country: "Vietnam",
  },
};

// Truy cập properties
console.log(user.name); // "John" - dot notation
console.log(user["age"]); // 30 - bracket notation
console.log(user.address.city); // "Hanoi"

// Thêm/sửa/xóa properties
user.email = "john@example.com"; // thêm
user.age = 31; // sửa
delete user.isAdmin; // xóa

// 2. Array - Danh sách có thứ tự
let fruits = ["Apple", "Banana", "Orange"];
let mixed = [1, "two", true, null, { name: "John" }];

// Truy cập elements
console.log(fruits[0]); // "Apple"
console.log(fruits.length); // 3

// Array methods (sẽ học kỹ hơn ở phần sau)
fruits.push("Mango"); // thêm cuối
fruits.pop(); // xóa cuối
fruits.unshift("Grape"); // thêm đầu
fruits.shift(); // xóa đầu

// 3. Function - Cũng là object
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

// Kiểm tra Array
console.log(Array.isArray([])); // true
console.log(Array.isArray({})); // false

// Kiểm tra null
const value = null;
console.log(value === null); // true

// instanceof cho objects
console.log([] instanceof Array); // true
console.log({} instanceof Object); // true
console.log(new Date() instanceof Date); // true
```

---

## 4. Operators (Toán tử)

### 4.1 Arithmetic Operators

```javascript
// Cơ bản
let a = 10,
  b = 3;
console.log(a + b); // 13 - Cộng
console.log(a - b); // 7  - Trừ
console.log(a * b); // 30 - Nhân
console.log(a / b); // 3.333... - Chia
console.log(a % b); // 1  - Chia lấy dư (modulo)
console.log(a ** b); // 1000 - Lũy thừa (ES7)

// Increment/Decrement
let x = 5;
console.log(x++); // 5 (post-increment, trả về rồi mới tăng)
console.log(x); // 6
console.log(++x); // 7 (pre-increment, tăng rồi mới trả về)
console.log(x--); // 7
console.log(--x); // 5

// String concatenation
console.log("Hello" + " " + "World"); // "Hello World"
console.log("Price: " + 100); // "Price: 100"
```

### 4.2 Comparison Operators

```javascript
// So sánh
console.log(5 > 3); // true
console.log(5 < 3); // false
console.log(5 >= 5); // true
console.log(5 <= 4); // false

// Equality
console.log(5 == "5"); // true  (loose equality - có type coercion)
console.log(5 === "5"); // false (strict equality - không coercion)
console.log(5 != "5"); // false
console.log(5 !== "5"); // true

// ⚠️ Luôn dùng === và !== để tránh bugs
console.log(0 == false); // true
console.log(0 === false); // false
console.log(null == undefined); // true
console.log(null === undefined); // false
```

### 4.3 Logical Operators

```javascript
// AND (&&) - true nếu TẤT CẢ đều true
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false

// OR (||) - true nếu ÍT NHẤT MỘT true
console.log(true || false); // true
console.log(false || false); // false

// NOT (!) - Đảo ngược
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
console.log(0 ?? "default"); // 0 (0 không phải null/undefined)
console.log(0 || "default"); // "default" (0 là falsy)
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

// Nested ternary (nên hạn chế)
let score = 85;
let grade = score >= 90 ? "A" : score >= 80 ? "B" : score >= 70 ? "C" : "F";
console.log(grade); // "B"
```

---

## 5. Control Flow (Luồng điều khiển)

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

// Shorthand cho simple conditions
let isRaining = true;
if (isRaining) console.log("Take an umbrella!");

// Với logical operators
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

// ⚠️ Đừng quên break, nếu không sẽ "fall through"
let fruit = "apple";
switch (fruit) {
  case "apple":
    console.log("Apple");
  // không có break → tiếp tục case tiếp theo
  case "banana":
    console.log("Banana");
    break;
  default:
    console.log("Other");
}
// Output: "Apple" và "Banana"
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

// Với strings
for (let char of "Hello") {
  console.log(char); // "H", "e", "l", "l", "o"
}

// Lấy cả index
for (let [index, color] of colors.entries()) {
  console.log(`${index}: ${color}`);
}
```

#### for...in

```javascript
// Iterate over object keys
let user = { name: "John", age: 30, city: "Hanoi" };

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}
// "name: John", "age: 30", "city: Hanoi"

// ⚠️ Tránh dùng for...in với arrays
let arr = ["a", "b", "c"];
for (let index in arr) {
  console.log(index); // "0", "1", "2" (strings, không phải numbers!)
}
```

#### while & do...while

```javascript
// while - Kiểm tra điều kiện trước
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

// do...while - Chạy ít nhất 1 lần
let num = 10;
do {
  console.log(num); // 10 (chạy 1 lần dù điều kiện false)
  num++;
} while (num < 5);
```

### 5.4 break & continue

```javascript
// break - Thoát khỏi loop
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0, 1, 2, 3, 4
}

// continue - Bỏ qua iteration hiện tại
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0, 1, 3, 4
}

// Label - Cho nested loops
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer;
    console.log(`${i}, ${j}`);
  }
}
```

---

## 6. Functions (Hàm)

### 6.1 Function Declaration

```javascript
// Cách khai báo cơ bản
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("John")); // "Hello, John!"

// Hoisting - function declarations được hoisted
sayHi(); // ✅ Works!
function sayHi() {
  console.log("Hi!");
}
```

### 6.2 Function Expression

```javascript
// Gán function vào biến
const greet = function (name) {
  return `Hello, ${name}!`;
};

// Không được hoisted
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

// 1 parameter - không cần parentheses
const double = (x) => x * 2;

// Không có parameter
const sayHello = () => "Hello!";

// Return object literal - cần parentheses
const createUser = (name, age) => ({ name, age });

// ⚠️ Arrow functions không có:
// - this riêng (inherit từ parent scope)
// - arguments object
// - Không thể dùng làm constructor
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

// arguments object (không dùng với arrow functions)
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

// Không có return → trả về undefined
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
// Function được gọi ngay khi định nghĩa
(function () {
  console.log("IIFE executed!");
})();

// Với arrow function
(() => {
  console.log("Arrow IIFE");
})();

// Với parameters
(function (name) {
  console.log(`Hello, ${name}!`);
})("John");

// Use case: Tạo private scope
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

## 7. Arrays (Mảng)

### 7.1 Tạo và truy cập Arrays

```javascript
// Tạo array
const fruits = ["Apple", "Banana", "Orange"];
const numbers = new Array(1, 2, 3); // Ít dùng
const empty = [];

// Truy cập elements
console.log(fruits[0]); // "Apple"
console.log(fruits[2]); // "Orange"
console.log(fruits[-1]); // undefined (không support negative)
console.log(fruits.at(-1)); // "Orange" (ES2022)

// Thay đổi element
fruits[1] = "Mango";
console.log(fruits); // ["Apple", "Mango", "Orange"]

// Length
console.log(fruits.length); // 3
```

### 7.2 Basic Array Methods

```javascript
let arr = [1, 2, 3];

// Thêm/xóa cuối
arr.push(4); // [1, 2, 3, 4] - trả về length mới
arr.pop(); // [1, 2, 3] - trả về element đã xóa

// Thêm/xóa đầu
arr.unshift(0); // [0, 1, 2, 3]
arr.shift(); // [1, 2, 3]

// Splice - thêm/xóa ở vị trí bất kỳ
arr.splice(1, 1); // Xóa 1 element từ index 1 → [1, 3]
arr.splice(1, 0, 2); // Thêm 2 tại index 1 → [1, 2, 3]
arr.splice(1, 1, "two"); // Thay thế → [1, "two", 3]

// Slice - cắt array (không thay đổi original)
let sliced = [1, 2, 3, 4, 5].slice(1, 4); // [2, 3, 4]

// Concat - nối arrays
let combined = [1, 2].concat([3, 4]); // [1, 2, 3, 4]

// Join - chuyển thành string
console.log(["a", "b", "c"].join("-")); // "a-b-c"

// Includes - kiểm tra có chứa không
console.log([1, 2, 3].includes(2)); // true

// IndexOf / LastIndexOf
console.log([1, 2, 3, 2].indexOf(2)); // 1
console.log([1, 2, 3, 2].lastIndexOf(2)); // 3

// Reverse & Sort (thay đổi original array!)
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

## 8. Objects (Đối tượng)

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

// Truy cập properties
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

// Thêm property
user.email = "john@example.com";
user["phone"] = "123456";

// Xóa property
delete user.phone;

// Kiểm tra property tồn tại
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
    city: "Hanoi",
    country: "Vietnam",
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
console.log(city); // "Hanoi"

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
    console.log(`Hello, I'm ${this.name}`); // 'this' là outer scope
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

// Callback với error handling
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

## 12. Bài tập thực hành

### Bài tập 1: Variables & Data Types

```javascript
// 1.1: Khai báo biến với các kiểu dữ liệu khác nhau
// - Tạo biến lưu tên (string), tuổi (number), đã kết hôn (boolean)
// - Tạo object chứa thông tin sản phẩm (name, price, inStock)
// - Tạo array chứa 5 loại trái cây

// 1.2: Type conversion
// - Convert string "123" thành number
// - Convert number 456 thành string
// - Check type của các biến trên
```

### Bài tập 2: Control Flow

```javascript
// 2.1: Viết function kiểm tra số chẵn/lẻ
function isEven(n) {
  // Implement
}

// 2.2: Viết function tính grade từ điểm số
function getGrade(score) {
  // A: 90-100, B: 80-89, C: 70-79, D: 60-69, F: < 60
}

// 2.3: FizzBuzz - In số từ 1-100
// - Nếu chia hết 3: "Fizz"
// - Nếu chia hết 5: "Buzz"
// - Nếu chia hết cả 3 và 5: "FizzBuzz"
// - Còn lại: in số
```

### Bài tập 3: Functions

```javascript
// 3.1: Viết function tính giai thừa
function factorial(n) {
  // Implement (cả recursion và loop)
}

// 3.2: Viết function tìm số lớn nhất trong array
function findMax(arr) {
  // Implement
}

// 3.3: Viết counter module với closure
function createCounter() {
  // Return object với increment, decrement, getCount, reset
}
```

### Bài tập 4: Arrays

```javascript
// 4.1: Cho array số, trả về array chỉ chứa số chẵn, mỗi số nhân 2
// Input: [1, 2, 3, 4, 5]
// Output: [4, 8]

// 4.2: Tính tổng tuổi của tất cả users
const users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 25 },
  { name: "Bob", age: 35 },
];

// 4.3: Loại bỏ duplicate từ array
// Input: [1, 2, 2, 3, 3, 3]
// Output: [1, 2, 3]
```

### Bài tập 5: Objects

```javascript
// 5.1: Deep clone object
function deepClone(obj) {
  // Implement (không dùng JSON.parse/stringify)
}

// 5.2: Merge nhiều objects
function mergeObjects(...objects) {
  // Implement
}

// 5.3: Tạo class Student với methods
class Student {
  // Properties: name, grades (array)
  // Methods: addGrade, getAverage, getStatus
}
```

### Bài tập 6: DOM & Events

```javascript
// 6.1: Tạo Todo List đơn giản
// - Input để thêm task
// - Button để add task
// - List hiển thị tasks
// - Click task để mark complete (strikethrough)
// - Button xóa task

// 6.2: Tạo Counter với UI
// - Display hiển thị số
// - Button tăng, giảm, reset
// - Không cho số âm
```

---

## 13. Tài liệu tham khảo

- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [W3Schools JavaScript Tutorial](https://www.w3schools.com/js/)
- [freeCodeCamp JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

---

## 14. Tổng kết

### Những điểm quan trọng cần nhớ:

1. **Variables:**

   - Dùng `const` mặc định, `let` khi cần reassign
   - Tránh dùng `var`

2. **Data Types:**

   - 7 primitive types: string, number, boolean, null, undefined, symbol, bigint
   - Reference types: object, array, function
   - Dùng `===` thay vì `==`

3. **Functions:**

   - Function declaration được hoisted
   - Arrow functions không có `this` riêng
   - Dùng default parameters và rest parameters

4. **Arrays:**

   - `map`, `filter`, `reduce` là 3 methods quan trọng nhất
   - Spread operator để copy và merge arrays
   - Destructuring để extract values

5. **Objects:**

   - Dot notation vs bracket notation
   - Object methods: keys, values, entries
   - Destructuring để extract properties

6. **Error Handling:**

   - Luôn dùng try-catch cho risky operations
   - Optional chaining (`?.`) để tránh errors

7. **Async:**
   - Callbacks → Promises → Async/Await
   - Prefer async/await cho code dễ đọc

**Lời khuyên:** Practice, practice, practice! Kiến thức JavaScript cơ bản là nền tảng cho mọi framework và library JavaScript khác.

# Module 1: Preparation - Chuẩn bị

## Mục tiêu module

Module này giúp bạn làm quen với môi trường làm việc, các công cụ cần thiết và nền tảng JavaScript cơ bản.

---

## 1. Giới thiệu về công ty (Introduce about the company)

### Khái niệm

- Tìm hiểu về tầm nhìn, sứ mệnh và văn hóa công ty
- Làm quen với quy trình làm việc và team structure
- Hiểu rõ sản phẩm/dịch vụ công ty đang phát triển

### Ví dụ

- Tham gia buổi orientation
- Đọc tài liệu giới thiệu công ty
- Gặp gỡ các team members

---

## 2. Tool và Equipments

### Khái niệm

Các công cụ cần thiết cho công việc phát triển phần mềm:

- **IDE/Code Editor**: VS Code, WebStorm
- **Version Control**: Git, GitHub/GitLab
- **Browser DevTools**: Chrome DevTools, Firefox Developer Tools
- **Design Tools**: Figma, Adobe XD
- **Communication**: Slack, Microsoft Teams

### Ví dụ

```bash
# Cài đặt VS Code
# Download từ: https://code.visualstudio.com/

# Cài đặt Git
brew install git  # macOS
# hoặc download từ: https://git-scm.com/

# Cài đặt Node.js
brew install node  # macOS
# hoặc download từ: https://nodejs.org/
```

**Extensions hữu ích cho VS Code:**

- ESLint
- Prettier
- GitLens
- Auto Rename Tag
- Live Server

---

## 3. Git Flow Process

### 3.1 Basic Git Flow

#### Khái niệm

Git Flow là một workflow quản lý code với các nhánh (branches) được tổ chức rõ ràng.

**Các nhánh chính:**

- `main/master`: Code production, ổn định nhất
- `develop`: Code đang phát triển
- `feature/*`: Phát triển tính năng mới
- `hotfix/*`: Sửa bug khẩn cấp trên production
- `release/*`: Chuẩn bị release version mới

#### Ví dụ

```bash
# Clone repository
git clone https://github.com/company/project.git
cd project

# Tạo feature branch từ develop
git checkout develop
git pull origin develop
git checkout -b feature/user-authentication

# Làm việc và commit
git add .
git commit -m "feat: implement user login form"

# Push lên remote
git push origin feature/user-authentication

# Tạo Pull Request trên GitHub/GitLab
```

### 3.2 Sử dụng git stash

#### Khái niệm

`git stash` cho phép tạm thời lưu các thay đổi chưa commit để chuyển sang branch khác.

#### Ví dụ

```bash
# Đang code trên feature branch, cần chuyển sang fix bug
git stash save "WIP: working on login form"

# Chuyển sang branch khác
git checkout hotfix/critical-bug

# Quay lại và restore stash
git checkout feature/user-authentication
git stash pop  # Apply và xóa stash
# hoặc
git stash apply  # Apply nhưng giữ stash

# Xem danh sách stash
git stash list

# Xóa stash
git stash drop stash@{0}
```

### 3.3 git reset

#### Khái niệm

`git reset` dùng để undo commits hoặc unstage files.

**Các mode:**

- `--soft`: Giữ changes trong staging area
- `--mixed` (default): Giữ changes trong working directory
- `--hard`: Xóa hoàn toàn changes

#### Ví dụ

```bash
# Undo commit gần nhất, giữ changes
git reset --soft HEAD~1

# Undo commit và unstage files
git reset HEAD~1
# hoặc
git reset --mixed HEAD~1

# Undo commit và xóa changes (NGUY HIỂM!)
git reset --hard HEAD~1

# Reset về một commit cụ thể
git reset --hard abc1234
```

### 3.4 git cherry-pick

#### Khái niệm

`git cherry-pick` cho phép apply một commit cụ thể từ branch khác vào branch hiện tại.

#### Ví dụ

```bash
# Tìm commit hash cần pick
git log --oneline

# Cherry-pick một commit
git cherry-pick abc1234

# Cherry-pick nhiều commits
git cherry-pick abc1234 def5678

# Cherry-pick một range
git cherry-pick abc1234..def5678

# Cherry-pick nhưng không auto-commit
git cherry-pick -n abc1234
```

### 3.5 Phân biệt git merge, git pull và git rebase

#### Khái niệm

**git merge**: Kết hợp hai branches, tạo merge commit

- Giữ nguyên lịch sử của cả hai branches
- Tạo một commit mới để merge

**git pull**: Fetch + Merge từ remote về local

- `git pull = git fetch + git merge`

**git rebase**: Đặt lại base của branch, tạo lịch sử tuyến tính

- Viết lại history
- Tạo commit history sạch hơn

#### Ví dụ

```bash
# GIT MERGE
git checkout develop
git merge feature/login
# Tạo merge commit: "Merge branch 'feature/login' into develop"

# GIT PULL
git checkout develop
git pull origin develop
# Tương đương:
# git fetch origin develop
# git merge origin/develop

# GIT REBASE
git checkout feature/login
git rebase develop
# Di chuyển các commits của feature/login lên trên develop

# Rebase interactive để squash commits
git rebase -i HEAD~3
```

**Khi nào dùng merge vs rebase?**

- **Merge**: Khi cần giữ nguyên history, làm việc team
- **Rebase**: Khi muốn history sạch, làm việc local branch

---

## 4. JSCore

### Khái niệm

JSCore là các khái niệm nền tảng của JavaScript:

#### 4.1 Types & Grammar

**Primitive Types:**

- `string`
- `number`
- `boolean`
- `undefined`
- `null`
- `symbol` (ES6)
- `bigint` (ES2020)

**Reference Types:**

- `object`
- `array`
- `function`

#### Ví dụ

```javascript
// Primitive types
let name = "John"; // string
let age = 25; // number
let isStudent = true; // boolean
let job; // undefined
let salary = null; // null
let id = Symbol("id"); // symbol
let bigNumber = 123456789n; // bigint

// Reference types
let person = {
  // object
  name: "John",
  age: 25,
};
let numbers = [1, 2, 3]; // array
let greet = function () {}; // function

// Type checking
console.log(typeof name); // "string"
console.log(typeof age); // "number"
console.log(typeof isStudent); // "boolean"
console.log(typeof job); // "undefined"
console.log(typeof salary); // "object" (quirk!)
console.log(typeof person); // "object"
console.log(Array.isArray(numbers)); // true
```

#### 4.2 Variables, if/else, operators, boolean logic

#### Ví dụ

```javascript
// Variables
var oldWay = "var is function-scoped";
let modern = "let is block-scoped";
const constant = "const cannot be reassigned";

// if/else
let score = 85;
if (score >= 90) {
  console.log("Excellent");
} else if (score >= 70) {
  console.log("Good");
} else {
  console.log("Need improvement");
}

// Operators
let sum = 5 + 3; // Addition
let diff = 10 - 4; // Subtraction
let product = 4 * 5; // Multiplication
let quotient = 20 / 4; // Division
let remainder = 10 % 3; // Modulus
let power = 2 ** 3; // Exponentiation (ES2016)

// Comparison operators
console.log(5 == "5"); // true (loose equality)
console.log(5 === "5"); // false (strict equality)
console.log(5 != "5"); // false
console.log(5 !== "5"); // true

// Boolean logic
let isAdult = age >= 18;
let hasLicense = true;
let canDrive = isAdult && hasLicense; // AND
let canEnter = isAdult || hasTicket; // OR
let isNotStudent = !isStudent; // NOT
```

#### 4.3 Functions, Arrays, Objects, Loops, Strings

#### Ví dụ

```javascript
// FUNCTIONS
// Function declaration
function greet(name) {
  return `Hello, ${name}!`;
}

// Function expression
const add = function (a, b) {
  return a + b;
};

// Arrow function (ES6)
const multiply = (a, b) => a * b;

// ARRAYS
let fruits = ["apple", "banana", "orange"];

// Array methods
fruits.push("grape"); // Add to end
fruits.pop(); // Remove from end
fruits.unshift("mango"); // Add to start
fruits.shift(); // Remove from start

// Array iteration
fruits.forEach((fruit) => console.log(fruit));
let upperFruits = fruits.map((fruit) => fruit.toUpperCase());
let longFruits = fruits.filter((fruit) => fruit.length > 5);
let totalLength = fruits.reduce((sum, fruit) => sum + fruit.length, 0);

// OBJECTS
let student = {
  name: "Alice",
  age: 20,
  grades: [85, 90, 92],
  getAverage: function () {
    return this.grades.reduce((a, b) => a + b) / this.grades.length;
  },
};

// Access properties
console.log(student.name); // Dot notation
console.log(student["age"]); // Bracket notation
console.log(student.getAverage()); // Call method

// LOOPS
// for loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for...of (iterate values)
for (let fruit of fruits) {
  console.log(fruit);
}

// for...in (iterate keys)
for (let key in student) {
  console.log(`${key}: ${student[key]}`);
}

// while loop
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

// STRINGS
let message = "Hello, World!";
console.log(message.length); // 13
console.log(message.toUpperCase()); // "HELLO, WORLD!"
console.log(message.toLowerCase()); // "hello, world!"
console.log(message.includes("World")); // true
console.log(message.split(", ")); // ["Hello", "World!"]
console.log(message.slice(0, 5)); // "Hello"
console.log(message.replace("World", "JavaScript")); // "Hello, JavaScript!"

// Template literals (ES6)
let name = "Alice";
let greeting = `Hello, ${name}! You are ${age} years old.`;
```

---

## 5. Scope & Closures

### 5.1 Scope

#### Khái niệm

Scope xác định phạm vi truy cập của biến trong JavaScript.

**Các loại scope:**

- **Global Scope**: Biến có thể truy cập ở mọi nơi
- **Function Scope**: Biến chỉ truy cập được trong function (`var`)
- **Block Scope**: Biến chỉ truy cập được trong block `{}` (`let`, `const`)
- **Lexical Scope**: Function có thể truy cập biến của outer function
- **Dynamic Scope**: JavaScript không có dynamic scope

#### Ví dụ

```javascript
// Global scope
let globalVar = "I'm global";

function outerFunction() {
  // Function scope
  var functionVar = "I'm in function";

  if (true) {
    // Block scope
    let blockVar = "I'm in block";
    const blockConst = "I'm also in block";
    var notBlockScoped = "var ignores block";

    console.log(globalVar); // ✓ Can access
    console.log(functionVar); // ✓ Can access
    console.log(blockVar); // ✓ Can access
  }

  console.log(globalVar); // ✓ Can access
  console.log(functionVar); // ✓ Can access
  // console.log(blockVar);      // ✗ ReferenceError
  console.log(notBlockScoped); // ✓ Can access (var!)
}

// console.log(functionVar);     // ✗ ReferenceError

// Lexical Scope
function outer() {
  let outerVar = "outer";

  function inner() {
    let innerVar = "inner";
    console.log(outerVar); // ✓ Can access parent scope
    console.log(innerVar); // ✓ Can access own scope
  }

  inner();
  // console.log(innerVar); // ✗ Cannot access child scope
}
```

### 5.2 Closures

#### Khái niệm

Closure là một function có thể truy cập biến từ outer scope ngay cả khi outer function đã return.

**Đặc điểm:**

- Function bên trong có thể "nhớ" và truy cập scope của function bên ngoài
- Tạo private variables
- Sử dụng trong callbacks, event handlers

#### Ví dụ

```javascript
// Basic closure
function createCounter() {
  let count = 0; // Private variable

  return {
    increment: function () {
      count++;
      return count;
    },
    decrement: function () {
      count--;
      return count;
    },
    getCount: function () {
      return count;
    },
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
console.log(counter.count); // undefined (private!)

// Closure in callbacks
function setupButton() {
  let clickCount = 0;

  document.getElementById("btn").addEventListener("click", function () {
    clickCount++;
    console.log(`Button clicked ${clickCount} times`);
  });
}

// Closure for data privacy
function createBankAccount(initialBalance) {
  let balance = initialBalance;

  return {
    deposit: function (amount) {
      if (amount > 0) {
        balance += amount;
        return `Deposited ${amount}. New balance: ${balance}`;
      }
    },
    withdraw: function (amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return `Withdrew ${amount}. New balance: ${balance}`;
      }
      return "Insufficient funds";
    },
    getBalance: function () {
      return balance;
    },
  };
}

const myAccount = createBankAccount(1000);
console.log(myAccount.deposit(500)); // "Deposited 500. New balance: 1500"
console.log(myAccount.withdraw(200)); // "Withdrew 200. New balance: 1300"
console.log(myAccount.getBalance()); // 1300
// console.log(myAccount.balance);      // undefined (cannot access directly!)
```

---

## 6. This and Object Prototypes

### 6.1 Modular (Import, Export)

#### Khái niệm

ES6 Modules cho phép tách code thành các file riêng biệt và import/export.

**Export types:**

- Named Export: `export { name, age }`
- Default Export: `export default Component`

#### Ví dụ

```javascript
// utils.js - Named exports
export const PI = 3.14159;

export function add(a, b) {
  return a + b;
}

export class Calculator {
  multiply(a, b) {
    return a * b;
  }
}

// math.js - Default export
export default function subtract(a, b) {
  return a - b;
}

// app.js - Import
import subtract from "./math.js"; // Default import
import { PI, add, Calculator } from "./utils.js"; // Named imports
import * as Utils from "./utils.js"; // Import all

console.log(PI); // 3.14159
console.log(add(5, 3)); // 8
console.log(subtract(10, 4)); // 6

const calc = new Calculator();
console.log(calc.multiply(4, 5)); // 20

console.log(Utils.PI); // 3.14159
console.log(Utils.add(2, 3)); // 5

// Rename imports
import { add as sum } from "./utils.js";
console.log(sum(1, 2)); // 3
```

### 6.2 Async: callback, promise, async-await

#### Khái niệm

JavaScript là ngôn ngữ bất đồng bộ (asynchronous) để xử lý các tác vụ mất thời gian.

**3 cách xử lý async:**

1. **Callback**: Function truyền vào làm tham số
2. **Promise**: Object đại diện cho kết quả tương lai
3. **Async/Await**: Syntax sugar của Promise, dễ đọc hơn

#### Ví dụ

```javascript
// 1. CALLBACK
function fetchDataCallback(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "John" };
    callback(data);
  }, 1000);
}

fetchDataCallback((data) => {
  console.log("Data received:", data);
});

// Callback hell (Pyramid of Doom)
doSomething(function (result1) {
  doSomethingElse(result1, function (result2) {
    doAnotherThing(result2, function (result3) {
      console.log("Final result:", result3);
    });
  });
});

// 2. PROMISE
function fetchDataPromise() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const success = true;
      if (success) {
        resolve({ id: 1, name: "John" });
      } else {
        reject(new Error("Failed to fetch data"));
      }
    }, 1000);
  });
}

// Using Promise
fetchDataPromise()
  .then((data) => {
    console.log("Data:", data);
    return data.id;
  })
  .then((id) => {
    console.log("ID:", id);
  })
  .catch((error) => {
    console.error("Error:", error);
  })
  .finally(() => {
    console.log("Cleanup");
  });

// Promise chaining (better than callback hell)
doSomething()
  .then((result1) => doSomethingElse(result1))
  .then((result2) => doAnotherThing(result2))
  .then((result3) => console.log("Final result:", result3))
  .catch((error) => console.error(error));

// 3. ASYNC/AWAIT
async function fetchDataAsync() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ id: 1, name: "John" });
    }, 1000);
  });
}

// Using async/await
async function getUserData() {
  try {
    console.log("Fetching data...");
    const data = await fetchDataAsync();
    console.log("Data:", data);

    const id = data.id;
    console.log("ID:", id);

    return data;
  } catch (error) {
    console.error("Error:", error);
  } finally {
    console.log("Cleanup");
  }
}

getUserData();

// Async/await with multiple promises
async function fetchMultipleData() {
  try {
    // Sequential (chậm - 3 seconds total)
    const user = await fetchUser();
    const posts = await fetchPosts();
    const comments = await fetchComments();

    // Parallel (nhanh - 1 second total)
    const [user2, posts2, comments2] = await Promise.all([
      fetchUser(),
      fetchPosts(),
      fetchComments(),
    ]);

    console.log(user2, posts2, comments2);
  } catch (error) {
    console.error(error);
  }
}

// Real-world example: API call
async function fetchUserFromAPI(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Failed to fetch user:", error);
    throw error;
  }
}
```

### 6.3 Async Advance: event loop, callback queue, calltask

#### Khái niệm

JavaScript runtime hoạt động theo cơ chế:

- **Call Stack**: Nơi functions được thực thi (LIFO - Last In First Out)
- **Event Loop**: Kiểm tra call stack và callback queue
- **Callback Queue (Task Queue)**: Hàng đợi cho callbacks (setTimeout, events)
- **Microtask Queue**: Hàng đợi ưu tiên cao (Promise, async/await)

**Thứ tự thực thi:**

1. Synchronous code (Call Stack)
2. Microtasks (Promise callbacks)
3. Macrotasks (setTimeout, setInterval)

#### Ví dụ

```javascript
// Event Loop Demo
console.log("1: Start");

setTimeout(() => {
  console.log("2: setTimeout (Macrotask)");
}, 0);

Promise.resolve().then(() => {
  console.log("3: Promise (Microtask)");
});

console.log("4: End");

// Output:
// 1: Start
// 4: End
// 3: Promise (Microtask)
// 2: setTimeout (Macrotask)

// Complex example
console.log("Script start");

setTimeout(() => {
  console.log("setTimeout 1");
  Promise.resolve().then(() => {
    console.log("Promise in setTimeout");
  });
}, 0);

Promise.resolve()
  .then(() => {
    console.log("Promise 1");
    setTimeout(() => {
      console.log("setTimeout in Promise");
    }, 0);
  })
  .then(() => {
    console.log("Promise 2");
  });

console.log("Script end");

// Output:
// Script start
// Script end
// Promise 1
// Promise 2
// setTimeout 1
// Promise in setTimeout
// setTimeout in Promise

// Microtask vs Macrotask
async function asyncTask() {
  console.log("Async function start");

  await Promise.resolve();
  console.log("After await (Microtask)");
}

setTimeout(() => console.log("setTimeout (Macrotask)"), 0);
asyncTask();
console.log("Synchronous");

// Output:
// Async function start
// Synchronous
// After await (Microtask)
// setTimeout (Macrotask)
```

---

## Bài tập thực hành

### Bài 1: Git Flow

1. Clone một repository
2. Tạo feature branch
3. Commit changes với conventional commit message
4. Tạo Pull Request

### Bài 2: JavaScript Fundamentals

```javascript
// Viết function tính tổng các số chẵn trong array
function sumEvenNumbers(arr) {
  // Your code here
}

console.log(sumEvenNumbers([1, 2, 3, 4, 5, 6])); // Expected: 12

// Viết function tìm user theo id
const users = [
  { id: 1, name: "Alice", age: 25 },
  { id: 2, name: "Bob", age: 30 },
  { id: 3, name: "Charlie", age: 35 },
];

function findUserById(users, id) {
  // Your code here
}

console.log(findUserById(users, 2)); // Expected: { id: 2, name: "Bob", age: 30 }
```

### Bài 3: Closure

```javascript
// Tạo function tạo ra unique ID
function createIdGenerator() {
  // Your code here
}

const getId = createIdGenerator();
console.log(getId()); // 1
console.log(getId()); // 2
console.log(getId()); // 3
```

### Bài 4: Async/Await

```javascript
// Viết function fetch multiple users và return tổng age
async function getTotalAge(userIds) {
  // Your code here
  // Sử dụng Promise.all để fetch parallel
}

getTotalAge([1, 2, 3]).then((total) => console.log(total));
```

---

## Tài liệu tham khảo

1. [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
2. [JavaScript.info](https://javascript.info/)
3. [Git Documentation](https://git-scm.com/doc)
4. [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
5. [Eloquent JavaScript](https://eloquentjavascript.net/)

---

**Next Module:** [HTML, CSS →](./02-html-css.md)

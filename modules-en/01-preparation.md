# Module 1: Preparation

## Module Objectives

This module helps you get familiar with the work environment, necessary tools, and foundational JavaScript knowledge.

---

## 1. Company Introduction

### Concept

- Learn about the company's vision, mission, and culture
- Familiarize yourself with work processes and team structure
- Understand the products/services the company is developing

### Examples

- Attend orientation sessions
- Read company introduction materials
- Meet team members

---

## 2. Tools and Equipment

### Concept

Essential tools for software development work:

- **IDE/Code Editor**: VS Code, WebStorm
- **Version Control**: Git, GitHub/GitLab
- **Browser DevTools**: Chrome DevTools, Firefox Developer Tools
- **Design Tools**: Figma, Adobe XD
- **Communication**: Slack, Microsoft Teams

### Examples

```bash
# Install VS Code
# Download from: https://code.visualstudio.com/

# Install Git
brew install git  # macOS
# or download from: https://git-scm.com/

# Install Node.js
brew install node  # macOS
# or download from: https://nodejs.org/
```

**Useful VS Code Extensions:**

- ESLint
- Prettier
- GitLens
- Auto Rename Tag
- Live Server

---

## 3. Git Flow Process

### 3.1 Basic Git Flow

#### Concept

Git Flow is a workflow for managing code with clearly organized branches.

**Main branches:**

- `main/master`: Production code, most stable
- `develop`: Code under development
- `feature/*`: New feature development
- `hotfix/*`: Urgent bug fixes on production
- `release/*`: Preparing new version release

#### Examples

```bash
# Clone repository
git clone https://github.com/company/project.git
cd project

# Create feature branch from develop
git checkout develop
git pull origin develop
git checkout -b feature/user-authentication

# Work and commit
git add .
git commit -m "feat: implement user login form"

# Push to remote
git push origin feature/user-authentication

# Create Pull Request on GitHub/GitLab
```

### 3.2 Using git stash

#### Concept

`git stash` allows you to temporarily save uncommitted changes to switch to another branch.

#### Examples

```bash
# Coding on feature branch, need to switch to fix bug
git stash save "WIP: working on login form"

# Switch to another branch
git checkout hotfix/critical-bug

# Return and restore stash
git checkout feature/user-authentication
git stash pop  # Apply and remove stash
# or
git stash apply  # Apply but keep stash

# View stash list
git stash list

# Delete stash
git stash drop stash@{0}
```

### 3.3 git reset

#### Concept

`git reset` is used to undo commits or unstage files.

**Modes:**

- `--soft`: Keep changes in staging area
- `--mixed` (default): Keep changes in working directory
- `--hard`: Delete changes completely

#### Examples

```bash
# Undo latest commit, keep changes
git reset --soft HEAD~1

# Undo commit and unstage files
git reset HEAD~1
# or
git reset --mixed HEAD~1

# Undo commit and delete changes (DANGEROUS!)
git reset --hard HEAD~1

# Reset to a specific commit
git reset --hard abc1234
```

### 3.4 git cherry-pick

#### Concept

`git cherry-pick` allows you to apply a specific commit from another branch to the current branch.

#### Examples

```bash
# Find commit hash to pick
git log --oneline

# Cherry-pick one commit
git cherry-pick abc1234

# Cherry-pick multiple commits
git cherry-pick abc1234 def5678

# Cherry-pick a range
git cherry-pick abc1234..def5678

# Cherry-pick without auto-commit
git cherry-pick -n abc1234
```

### 3.5 Difference between git merge, git pull, and git rebase

#### Concept

**git merge**: Combines two branches, creates merge commit

- Preserves history of both branches
- Creates a new commit for merge

**git pull**: Fetch + Merge from remote to local

- `git pull = git fetch + git merge`

**git rebase**: Resets branch base, creates linear history

- Rewrites history
- Creates cleaner commit history

#### Examples

```bash
# GIT MERGE
git checkout develop
git merge feature/login
# Creates merge commit: "Merge branch 'feature/login' into develop"

# GIT PULL
git checkout develop
git pull origin develop
# Equivalent to:
# git fetch origin develop
# git merge origin/develop

# GIT REBASE
git checkout feature/login
git rebase develop
# Move commits of feature/login on top of develop

# Interactive rebase to squash commits
git rebase -i HEAD~3
```

**When to use merge vs rebase?**

- **Merge**: When you need to preserve history, working in team
- **Rebase**: When you want clean history, working on local branch

---

## 4. JSCore

### Concept

JSCore are the foundational concepts of JavaScript:

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

#### Examples

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

#### Examples

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

#### Examples

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

#### Concept

Scope determines the accessibility of variables in JavaScript.

**Types of scope:**

- **Global Scope**: Variable accessible everywhere
- **Function Scope**: Variable accessible only within function (`var`)
- **Block Scope**: Variable accessible only within block `{}` (`let`, `const`)
- **Lexical Scope**: Function can access variables from outer function
- **Dynamic Scope**: JavaScript doesn't have dynamic scope

#### Examples

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

#### Concept

Closure is a function that can access variables from outer scope even when the outer function has returned.

**Characteristics:**

- Inner function can "remember" and access outer function's scope
- Creates private variables
- Used in callbacks, event handlers

#### Examples

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

#### Concept

ES6 Modules allow splitting code into separate files with import/export.

**Export types:**

- Named Export: `export { name, age }`
- Default Export: `export default Component`

#### Examples

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

#### Concept

JavaScript is an asynchronous language to handle time-consuming tasks.

**3 ways to handle async:**

1. **Callback**: Function passed as parameter
2. **Promise**: Object representing future result
3. **Async/Await**: Syntax sugar for Promise, more readable

#### Examples

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
    // Sequential (slow - 3 seconds total)
    const user = await fetchUser();
    const posts = await fetchPosts();
    const comments = await fetchComments();

    // Parallel (fast - 1 second total)
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

#### Concept

JavaScript runtime operates with these mechanisms:

- **Call Stack**: Where functions execute (LIFO - Last In First Out)
- **Event Loop**: Checks call stack and callback queue
- **Callback Queue (Task Queue)**: Queue for callbacks (setTimeout, events)
- **Microtask Queue**: Higher priority queue (Promise, async/await)

**Execution order:**

1. Synchronous code (Call Stack)
2. Microtasks (Promise callbacks)
3. Macrotasks (setTimeout, setInterval)

#### Examples

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

## Practice Exercises

### Exercise 1: Git Flow

1. Clone a repository
2. Create a feature branch
3. Commit changes with conventional commit message
4. Create a Pull Request

### Exercise 2: JavaScript Fundamentals

```javascript
// Write function to sum even numbers in array
function sumEvenNumbers(arr) {
  // Your code here
}

console.log(sumEvenNumbers([1, 2, 3, 4, 5, 6])); // Expected: 12

// Write function to find user by id
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

### Exercise 3: Closure

```javascript
// Create function that generates unique IDs
function createIdGenerator() {
  // Your code here
}

const getId = createIdGenerator();
console.log(getId()); // 1
console.log(getId()); // 2
console.log(getId()); // 3
```

### Exercise 4: Async/Await

```javascript
// Write function to fetch multiple users and return total age
async function getTotalAge(userIds) {
  // Your code here
  // Use Promise.all to fetch in parallel
}

getTotalAge([1, 2, 3]).then((total) => console.log(total));
```

---

## References

1. [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
2. [JavaScript.info](https://javascript.info/)
3. [Git Documentation](https://git-scm.com/doc)
4. [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
5. [Eloquent JavaScript](https://eloquentjavascript.net/)

---

**Next Module:** [HTML, CSS →](./02-html-css.md)

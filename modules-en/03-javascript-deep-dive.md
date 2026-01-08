# Module 3: JavaScript Deep Dive

## Module Objectives

This module provides in-depth knowledge of JavaScript, based on content from the famous **"You Don't Know JS"** book series by Kyle Simpson. This module will help you deeply understand how JavaScript actually works under the hood.

---

## 1. Scope & Closures

### 1.1 What is Scope?

#### Concept

**Scope** is the set of rules that determine where variables are stored and how they are found later. This is one of the most fundamental paradigms of nearly all programming languages.

**Key Questions:**

- Where do variables "live"?
- How does the program find them when needed?

#### Compiler Theory

Despite JavaScript often being categorized as a "dynamic" or "interpreted" language, it is actually a **compiled language**. The JavaScript engine performs similar steps to traditional compilers:

1. **Tokenizing/Lexing:** Breaking up a string of characters into meaningful tokens

   ```
   var a = 2; → [var] [a] [=] [2] [;]
   ```

2. **Parsing:** Turning a stream of tokens into an AST (Abstract Syntax Tree)

   ```
   VariableDeclaration
   ├── Identifier (a)
   └── AssignmentExpression
       └── NumericLiteral (2)
   ```

3. **Code-Generation:** Turning the AST into executable code

```javascript
// JavaScript does NOT run code directly as many people think
// It compiles first (in microseconds) then executes

// Example: When encountering this code
var a = 2;

// Engine performs:
// 1. Compiler asks Scope: "Does variable 'a' exist?"
// 2. If not → Compiler tells Scope to create new variable
// 3. Then Engine runs: a = 2 (assignment)
```

### 1.2 Lexical Scope

#### Concept

**Lexical Scope** is scope defined at author-time, based on where functions and blocks are placed.

```javascript
function foo(a) {
  var b = a * 2;

  function bar(c) {
    console.log(a, b, c);
  }

  bar(b * 3);
}

foo(2); // 2, 4, 12

/*
Scope hierarchy:
┌─────────────────────────────────┐
│ Global Scope                     │
│   └─ foo                         │
│      ┌──────────────────────────┐│
│      │ foo Scope                ││
│      │   └─ a, b, bar           ││
│      │      ┌──────────────────┐││
│      │      │ bar Scope        │││
│      │      │   └─ c           │││
│      │      └──────────────────┘││
│      └──────────────────────────┘│
└─────────────────────────────────┘
*/
```

#### Scope Look-up

```javascript
// Engine searches for variables from inside out
function outer() {
  var x = 10;

  function inner() {
    var y = 20;
    console.log(x + y); // x found in outer scope
  }

  inner();
}

outer(); // 30
```

### 1.3 Function vs Block Scope

#### Function Scope

```javascript
// Each function creates a new scope
function doSomething(a) {
  var b = a + 1; // b only exists within this function

  function doMore(c) {
    var d = c - 1;
    console.log(a, b, c, d);
  }

  doMore(b * 2);
}

doSomething(2); // 2, 3, 6, 5
// console.log(b); // ❌ ReferenceError: b is not defined
```

#### Block Scope (ES6+)

```javascript
// let and const create block scope
function processItems() {
  var functionScoped = "accessible everywhere in function";

  if (true) {
    var varInBlock = "still function scoped!";
    let letInBlock = "block scoped";
    const constInBlock = "also block scoped";

    console.log(letInBlock); // ✅ OK
  }

  console.log(varInBlock); // ✅ "still function scoped!"
  // console.log(letInBlock); // ❌ ReferenceError
}

// Practical example: for loops
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i); // 3, 3, 3 (because var is function scoped)
  }, 100);
}

for (let j = 0; j < 3; j++) {
  setTimeout(function () {
    console.log(j); // 0, 1, 2 (because let is block scoped)
  }, 100);
}
```

### 1.4 Hoisting

#### Concept

**Hoisting** is the behavior where variable and function declarations are "moved" to the top of their scope during the compilation phase.

```javascript
// Code you write:
console.log(a); // undefined (not ReferenceError!)
var a = 2;

// JavaScript "sees" it like this:
var a; // Declaration is hoisted
console.log(a); // undefined
a = 2; // Assignment stays in place

// Function declarations are fully hoisted
foo(); // "Hello!" - Works!

function foo() {
  console.log("Hello!");
}

// Function expressions are NOT hoisted
bar(); // ❌ TypeError: bar is not a function

var bar = function () {
  console.log("World!");
};

// let/const also hoist but in "Temporal Dead Zone"
// console.log(x); // ❌ ReferenceError: Cannot access 'x' before initialization
let x = 10;
```

#### Priority Order

```javascript
foo(); // 1

var foo;

function foo() {
  console.log(1);
}

foo = function () {
  console.log(2);
};

// Functions are hoisted BEFORE variables
// Duplicate function declarations → later overrides earlier

function foo() {
  console.log(1);
}

function foo() {
  console.log(3); // This function overrides the previous one
}

foo(); // 3
```

### 1.5 Closures

#### Concept

> **Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.**

```javascript
function foo() {
  var a = 2;

  function bar() {
    console.log(a);
  }

  return bar;
}

var baz = foo();

baz(); // 2 -- This is closure!

/*
Explanation:
1. foo() finishes executing, normally garbage collector would clean up foo's scope
2. But bar() still holds a reference to that scope
3. So the scope isn't deleted, bar() can still access 'a'
4. This reference is called CLOSURE
*/
```

#### In Practice: Closures Are Everywhere

```javascript
// 1. Event handlers
function setupButton(name, selector) {
  $(selector).click(function activator() {
    console.log("Activating: " + name); // closure over 'name'
  });
}

// 2. setTimeout/setInterval
function wait(message) {
  setTimeout(function timer() {
    console.log(message); // closure over 'message'
  }, 1000);
}

wait("Hello, closure!");

// 3. Callbacks
function fetchData(url, callback) {
  const timestamp = Date.now();

  $.get(url, function (data) {
    console.log(`Fetched at ${timestamp}:`, data); // closure over 'timestamp'
    callback(data);
  });
}

// 4. Module Pattern
function createCounter() {
  var count = 0; // private variable

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

var counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.getCount(); // 2
// counter.count; // undefined - private!
```

#### Classic Closure Problem & Solution

```javascript
// ❌ Problem: Classic loop + closure issue
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i); // 6, 6, 6, 6, 6
  }, i * 1000);
}
// All closures share the same 'i', and when setTimeout runs, i = 6

// ✅ Solution 1: IIFE (Immediately Invoked Function Expression)
for (var i = 1; i <= 5; i++) {
  (function (j) {
    setTimeout(function timer() {
      console.log(j); // 1, 2, 3, 4, 5
    }, j * 1000);
  })(i);
}

// ✅ Solution 2: let (block scope)
for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i); // 1, 2, 3, 4, 5
  }, i * 1000);
}
// let creates a new binding for each iteration
```

---

## 2. this & Object Prototypes

### 2.1 What is `this`?

#### Concept

`this` is a binding created for each function invocation, and its value depends entirely on the **call-site** (where the function is called).

```javascript
// ❌ Common misconception #1: this refers to the function itself
function foo(num) {
  console.log("foo: " + num);
  this.count++; // Does NOT refer to foo.count!
}

foo.count = 0;

for (var i = 0; i < 10; i++) {
  foo(i);
}

console.log(foo.count); // 0 - still 0!

// ❌ Common misconception #2: this refers to function's scope
// → NEVER true in JavaScript
```

### 2.2 Four Rules for Determining `this`

#### Call-site & Call-stack

```javascript
function baz() {
  // call-stack: `baz`
  // call-site: global scope

  console.log("baz");
  bar(); // ← call-site for `bar`
}

function bar() {
  // call-stack: `baz` → `bar`
  // call-site: in `baz`

  console.log("bar");
  foo(); // ← call-site for `foo`
}

function foo() {
  // call-stack: `baz` → `bar` → `foo`
  // call-site: in `bar`

  console.log("foo");
}

baz(); // ← call-site for `baz`
```

#### Rule 1: Default Binding

```javascript
// When function is called "plain" - without context
function foo() {
  console.log(this.a);
}

var a = 2;

foo(); // 2 (in non-strict mode)
// this points to global object (window in browser)

// In strict mode:
function bar() {
  "use strict";
  console.log(this.a);
}

bar(); // TypeError: Cannot read property 'a' of undefined
```

#### Rule 2: Implicit Binding

```javascript
// When function is called with a context object
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

obj.foo(); // 2 - this = obj

// Only the last level matters
var obj1 = {
  a: 2,
  obj2: {
    a: 42,
    foo: foo,
  },
};

obj1.obj2.foo(); // 42 - this = obj2

// ⚠️ Implicitly Lost - Common pitfall!
var bar = obj.foo; // function reference
var a = "oops, global";

bar(); // "oops, global" - Lost implicit binding!

// ⚠️ Callbacks also lose binding
function doFoo(fn) {
  fn(); // ← call-site is plain call
}

doFoo(obj.foo); // "oops, global"

// setTimeout also
setTimeout(obj.foo, 100); // "oops, global"
```

#### Rule 3: Explicit Binding

```javascript
// Using call(), apply(), or bind()
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
};

foo.call(obj); // 2 - explicitly bind this = obj
foo.apply(obj); // 2 - same as call

// Difference between call and apply:
function greet(greeting, punctuation) {
  console.log(greeting + ", " + this.name + punctuation);
}

var person = { name: "Alice" };

greet.call(person, "Hello", "!"); // "Hello, Alice!"
greet.apply(person, ["Hi", "?"]); // "Hi, Alice?"

// Hard Binding with bind()
var bar = foo.bind(obj);

bar(); // 2
setTimeout(bar, 100); // 2 - doesn't lose binding
bar.call(window); // 2 - cannot override
```

#### Rule 4: `new` Binding

```javascript
// When function is called with 'new' keyword
function Foo(a) {
  this.a = a;
}

var bar = new Foo(2);
console.log(bar.a); // 2

// When calling new:
// 1. New object is created
// 2. Object is linked to prototype
// 3. Object is set as 'this' for function call
// 4. Returns that object (unless function returns another object)
```

#### Priority Order

```javascript
// new > explicit (call/apply/bind) > implicit > default

// 1. Is function called with new?
//    → this = newly created object

// 2. Is function called with call/apply/bind?
//    → this = specified object

// 3. Is function called with context object?
//    → this = that context object

// 4. Default:
//    → strict mode: this = undefined
//    → non-strict: this = global object

// Practical example
function foo(something) {
  this.a = something;
}

var obj1 = { foo: foo };
var obj2 = {};

obj1.foo(2);
console.log(obj1.a); // 2

obj1.foo.call(obj2, 3);
console.log(obj2.a); // 3

var bar = new obj1.foo(4);
console.log(obj1.a); // 2 (unchanged)
console.log(bar.a); // 4
```

### 2.3 Arrow Functions and Lexical `this`

```javascript
// Arrow functions do NOT have their own this
// They inherit this from enclosing scope (lexical this)

function foo() {
  // Arrow function captures 'this' from foo
  return (a) => {
    console.log(this.a);
  };
}

var obj1 = { a: 2 };
var obj2 = { a: 3 };

var bar = foo.call(obj1);
bar.call(obj2); // 2, not 3!
// Arrow function keeps this = obj1, cannot override

// Before ES6 - had to use self = this
function foo() {
  var self = this;
  setTimeout(function () {
    console.log(self.a); // using closure
  }, 100);
}

// ES6+ - arrow functions
function foo() {
  setTimeout(() => {
    console.log(this.a); // lexical this
  }, 100);
}
```

### 2.4 Prototypes

#### [[Prototype]] Chain

```javascript
// Every object has an internal property [[Prototype]]
// This is a reference to another object

var anotherObject = {
  a: 2,
};

// Create an object linked to anotherObject
var myObject = Object.create(anotherObject);

myObject.a; // 2 - found through prototype chain

/*
myObject
  [[Prototype]] → anotherObject
                    a: 2
                    [[Prototype]] → Object.prototype
                                      [[Prototype]] → null
*/

// for..in and 'in' operator also check prototype chain
for (var k in myObject) {
  console.log("found: " + k); // "found: a"
}

"a" in myObject; // true
```

#### "Class" Pattern in JavaScript

```javascript
// JavaScript does NOT have classes in the traditional sense
// Only objects linked to objects

function Foo(name) {
  this.name = name;
}

Foo.prototype.myName = function () {
  return this.name;
};

var a = new Foo("a");
var b = new Foo("b");

a.myName(); // "a"
b.myName(); // "b"

/*
Foo
  prototype → Foo.prototype
                myName: function
                constructor → Foo

a
  name: "a"
  [[Prototype]] → Foo.prototype

b
  name: "b"
  [[Prototype]] → Foo.prototype
*/
```

#### Prototypal Inheritance

```javascript
function Foo(name) {
  this.name = name;
}

Foo.prototype.myName = function () {
  return this.name;
};

function Bar(name, label) {
  Foo.call(this, name); // "super" constructor
  this.label = label;
}

// Create Bar.prototype linked to Foo.prototype
Bar.prototype = Object.create(Foo.prototype);

// Add own method
Bar.prototype.myLabel = function () {
  return this.label;
};

var a = new Bar("a", "obj a");

a.myName(); // "a"
a.myLabel(); // "obj a"

// ES6 class syntax (syntactic sugar)
class Foo {
  constructor(name) {
    this.name = name;
  }

  myName() {
    return this.name;
  }
}

class Bar extends Foo {
  constructor(name, label) {
    super(name);
    this.label = label;
  }

  myLabel() {
    return this.label;
  }
}
```

#### Behavior Delegation (OLOO Pattern)

```javascript
// Objects Linked to Other Objects - No "class" needed

var Task = {
  setID: function (ID) {
    this.id = ID;
  },
  outputID: function () {
    console.log(this.id);
  },
};

// XYZTask delegates to Task
var XYZTask = Object.create(Task);

XYZTask.prepareTask = function (ID, Label) {
  this.setID(ID);
  this.label = Label;
};

XYZTask.outputTaskDetails = function () {
  this.outputID();
  console.log(this.label);
};

// Usage
var task1 = Object.create(XYZTask);
task1.prepareTask(1, "Task One");
task1.outputTaskDetails(); // 1, "Task One"
```

---

## 3. Types & Coercion

### 3.1 Built-in Types

```javascript
// 7 primitive types + 1 object type
typeof undefined === "undefined"; // true
typeof true === "boolean"; // true
typeof 42 === "number"; // true
typeof "42" === "string"; // true
typeof { life: 42 } === "object"; // true
typeof Symbol() === "symbol"; // true (ES6)
typeof 42n === "bigint"; // true (ES2020)

// ⚠️ Quirks
typeof null === "object"; // true - Historical BUG!
typeof function a() {} === "function"; // true - subtype of object

// Checking for null
var a = null;
!a && typeof a === "object"; // true

// Arrays are also objects
typeof [1, 2, 3] === "object"; // true
Array.isArray([1, 2, 3]); // true
```

### 3.2 Values vs References

```javascript
// Primitives: passed by value
var a = 2;
var b = a; // copy of value
b++;
a; // 2 - unchanged

// Objects: passed by reference
var c = [1, 2, 3];
var d = c; // copy of reference
d.push(4);
c; // [1, 2, 3, 4] - same array!

// ⚠️ Re-assignment doesn't change original reference
function foo(x) {
  x.push(4);
  x; // [1, 2, 3, 4]

  x = [4, 5, 6]; // creates NEW reference
  x.push(7);
  x; // [4, 5, 6, 7]
}

var a = [1, 2, 3];
foo(a);
a; // [1, 2, 3, 4] - not [4, 5, 6, 7]
```

### 3.3 Coercion

#### ToBoolean - Falsy vs Truthy

```javascript
// ⚠️ There are EXACTLY 6 falsy values:
// • undefined
// • null
// • false
// • +0, -0, NaN
// • "" (empty string)

// EVERYTHING ELSE is truthy!
Boolean(undefined); // false
Boolean(null); // false
Boolean(0); // false
Boolean(""); // false
Boolean(NaN); // false

Boolean("0"); // true ← Non-empty string!
Boolean([]); // true ← Empty array is truthy!
Boolean({}); // true ← Empty object is truthy!
Boolean(function () {}); // true

// ⚠️ Common pitfall
if ([]) {
  console.log("Empty array is truthy!"); // Runs!
}

// But...
[] == false; // true! (complex coercion)
```

#### Explicit vs Implicit Coercion

```javascript
// EXPLICIT coercion - clear, readable
var a = 42;
var b = String(a); // "42"
var c = a.toString(); // "42"

var d = "3.14";
var e = Number(d); // 3.14
var f = +d; // 3.14 (unary + operator)
var g = parseInt(d); // 3 (integer only)

// IMPLICIT coercion - happens behind the scenes
var a = "42";
var b = a * 1; // 42 - string → number

var c = 42;
var d = c + ""; // "42" - number → string

// Boolean coercion
var a = 42;
var b = !!a; // true - explicit
if (a) {
} // implicit in condition
```

#### `==` vs `===`

```javascript
// ⚠️ COMMON misconception:
// "== checks value, === checks value AND type"

// ✅ CORRECT:
// "== allows coercion, === disallows coercion"

// If same type → both work the same
42 === 42; // true
42 == 42; // true

"42" === "42"; // true
"42" == "42"; // true

// Different types → == will coerce
"42" == 42; // true (string → number)
"42" === 42; // false

// ⚠️ Edge cases to remember
null == undefined; // true (special case)
null === undefined; // false

NaN == NaN; // false (NaN doesn't equal itself!)
NaN === NaN; // false

// Recommendations:
// • Use === when unsure about types
// • Use == when you know types and want coercion
// • ALWAYS use === with null/undefined
```

#### Coercion Best Practices

```javascript
// ✅ Good - explicit coercion
var num = Number(userInput);
var str = String(someValue);
var bool = Boolean(condition);

// ✅ Good - safe implicit coercion
var str = someNumber + "";
var num = +someString;

// ❌ Avoid - confusing implicit coercion
if ([] == false) {
} // true but confusing
if ("0" == false) {
} // true but confusing

// ✅ Better
if (Array.isArray(arr) && arr.length === 0) {
}
if (str === "0") {
}
```

---

## 4. Async & Performance

### 4.1 Event Loop

#### Concept

JavaScript is single-threaded, but can handle async operations through the **Event Loop**.

```javascript
// Call Stack, Web APIs, Callback Queue, Event Loop

console.log("First");

setTimeout(function () {
  console.log("Second");
}, 0);

console.log("Third");

// Output: "First", "Third", "Second"

/*
1. console.log("First") → Call Stack → execute → "First"
2. setTimeout → Call Stack → Web API (timer) → dequeue
3. console.log("Third") → Call Stack → execute → "Third"
4. Timer done → Callback Queue
5. Event Loop: Stack empty? → Move callback to Stack
6. Callback execute → "Second"
*/
```

#### Job Queue (Microtasks)

```javascript
// Promises use Job Queue (microtasks)
// Microtasks have priority over macrotasks (setTimeout, setInterval)

console.log("Script start");

setTimeout(function () {
  console.log("setTimeout");
}, 0);

Promise.resolve()
  .then(function () {
    console.log("Promise 1");
  })
  .then(function () {
    console.log("Promise 2");
  });

console.log("Script end");

// Output:
// "Script start"
// "Script end"
// "Promise 1"
// "Promise 2"
// "setTimeout"
```

### 4.2 Callbacks

```javascript
// Callback Hell / Pyramid of Doom
getData(function (a) {
  getMoreData(a, function (b) {
    getMoreData(b, function (c) {
      getMoreData(c, function (d) {
        getMoreData(d, function (e) {
          // ...
        });
      });
    });
  });
});

// Inversion of Control Problem
// When passing a callback, you DON'T control:
// • Is the callback called at the right time?
// • Is the callback called the right number of times?
// • Are the right arguments passed to the callback?
// • Are errors handled properly?

// Example: Third-party analytics
analytics.track(paymentData, function () {
  chargeCreditCard(); // Do you trust them to call exactly once?
});
```

### 4.3 Promises

#### Concept

**Promise** is a placeholder for a future value - a mechanism for handling async operations with more guarantees than callbacks.

```javascript
// Promise states:
// • Pending: waiting
// • Fulfilled: succeeded with a value
// • Rejected: failed with a reason

// Creating a Promise
var p = new Promise(function(resolve, reject) {
  // async operation
  setTimeout(function() {
    if (/* success */) {
      resolve("Success value");
    } else {
      reject("Error reason");
    }
  }, 1000);
});

// Consuming a Promise
p.then(
  function fulfilled(value) {
    console.log(value);
  },
  function rejected(reason) {
    console.error(reason);
  }
);
```

#### Promise Chain Flow

```javascript
// Each then() returns a NEW Promise
// Return value from callback = fulfillment value of new Promise

Promise.resolve(21)
  .then(function (v) {
    console.log(v); // 21
    return v * 2; // fulfill with 42
  })
  .then(function (v) {
    console.log(v); // 42
    // return a Promise for async step
    return new Promise(function (resolve) {
      setTimeout(function () {
        resolve(v * 2);
      }, 100);
    });
  })
  .then(function (v) {
    console.log(v); // 84 (after 100ms delay)
  });

// Real-world example
fetchUser(userId)
  .then(function (user) {
    return fetchUserPosts(user.id);
  })
  .then(function (posts) {
    return renderPosts(posts);
  })
  .then(function (html) {
    document.body.innerHTML = html;
  })
  .catch(function (err) {
    console.error("Something failed:", err);
  });
```

#### Promise Patterns

```javascript
// Promise.all - Wait for ALL to complete
// Fail fast: reject as soon as 1 Promise rejects

Promise.all([fetchUser(1), fetchUser(2), fetchUser(3)])
  .then(function (users) {
    console.log(users); // [user1, user2, user3]
  })
  .catch(function (err) {
    console.error("At least one failed:", err);
  });

// Promise.race - Wait for FIRST to settle
// Useful for timeouts

function fetchWithTimeout(url, timeout) {
  return Promise.race([
    fetch(url),
    new Promise(function (_, reject) {
      setTimeout(function () {
        reject(new Error("Timeout!"));
      }, timeout);
    }),
  ]);
}

// Promise.allSettled (ES2020) - Wait for ALL to settle (fulfill or reject)
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3),
]).then(function (results) {
  // [
  //   { status: "fulfilled", value: 1 },
  //   { status: "rejected", reason: "error" },
  //   { status: "fulfilled", value: 3 }
  // ]
});

// Promise.any (ES2021) - Wait for FIRST to fulfill
Promise.any([
  Promise.reject("error1"),
  Promise.resolve(2),
  Promise.resolve(3),
]).then(function (value) {
  console.log(value); // 2
});
```

### 4.4 Async/Await

```javascript
// Syntactic sugar over Promises
// Write async code that looks like sync code

async function fetchData() {
  try {
    const user = await fetchUser(userId);
    const posts = await fetchUserPosts(user.id);
    const html = await renderPosts(posts);
    document.body.innerHTML = html;
  } catch (err) {
    console.error("Something failed:", err);
  }
}

// Parallel execution with async/await
async function fetchAllUsers() {
  // ❌ Sequential - slow
  const user1 = await fetchUser(1);
  const user2 = await fetchUser(2);
  const user3 = await fetchUser(3);

  // ✅ Parallel - fast
  const [user1, user2, user3] = await Promise.all([
    fetchUser(1),
    fetchUser(2),
    fetchUser(3),
  ]);
}

// Error handling patterns
async function handleErrors() {
  // Pattern 1: try-catch
  try {
    const result = await riskyOperation();
  } catch (err) {
    handleError(err);
  }

  // Pattern 2: .catch() on Promise
  const result = await riskyOperation().catch(handleError);

  // Pattern 3: Wrapper function
  const [error, result] = await to(riskyOperation());
  if (error) {
    handleError(error);
  }
}

// Utility function
function to(promise) {
  return promise.then((data) => [null, data]).catch((err) => [err, null]);
}
```

---

## 5. ES6+ Features

### 5.1 let & const

```javascript
// Block scope
{
  let a = 1;
  const b = 2;
}
// console.log(a); // ReferenceError

// Temporal Dead Zone (TDZ)
// console.log(x); // ReferenceError
let x = 10;

// const = constant REFERENCE (not value!)
const arr = [1, 2, 3];
arr.push(4); // ✅ OK - mutating is allowed
// arr = [5, 6]; // ❌ Error - reassignment not allowed

const obj = { a: 1 };
obj.a = 2; // ✅ OK
// obj = {};     // ❌ Error

// For immutable objects, use Object.freeze()
const frozen = Object.freeze({ a: 1 });
frozen.a = 2; // Silent fail in non-strict, Error in strict
```

### 5.2 Arrow Functions

```javascript
// Concise syntax
const add = (a, b) => a + b;
const square = (x) => x * x;
const greet = () => "Hello!";

// Multi-line body needs braces and explicit return
const process = (x) => {
  const y = x * 2;
  return y + 1;
};

// Arrow functions limitations:
// 1. No 'this' binding
// 2. No 'arguments' object
// 3. Cannot be used as constructors
// 4. No 'prototype' property

// ❌ Wrong use case
const obj = {
  name: "Alice",
  greet: () => {
    console.log(this.name); // undefined!
  },
};

// ✅ Use regular function for methods
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name); // "Alice"
  },
};
```

### 5.3 Template Literals

```javascript
// String interpolation
const name = "World";
const greeting = `Hello, ${name}!`;

// Multi-line strings
const html = `
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
`;

// Expression evaluation
const a = 5;
const b = 10;
console.log(`Sum: ${a + b}, Product: ${a * b}`);

// Tagged templates
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    return result + str + (values[i] ? `<mark>${values[i]}</mark>` : "");
  }, "");
}

const name = "JavaScript";
const output = highlight`Welcome to ${name} course!`;
// "Welcome to <mark>JavaScript</mark> course!"
```

### 5.4 Destructuring

```javascript
// Array destructuring
const [a, b, ...rest] = [1, 2, 3, 4, 5];
// a = 1, b = 2, rest = [3, 4, 5]

const [first, , third] = [1, 2, 3]; // Skip elements
// first = 1, third = 3

// Default values
const [x = 0, y = 0] = [1];
// x = 1, y = 0

// Swap variables
let a = 1,
  b = 2;
[a, b] = [b, a];
// a = 2, b = 1

// Object destructuring
const { name, age } = { name: "Alice", age: 25 };

// Rename variables
const { name: userName, age: userAge } = user;

// Default values
const { title = "Untitled" } = article;

// Nested destructuring
const {
  address: { city, zip = "00000" },
} = user;

// Function parameters
function greet({ name, greeting = "Hello" }) {
  console.log(`${greeting}, ${name}!`);
}

greet({ name: "Alice" }); // "Hello, Alice!"
```

### 5.5 Spread & Rest Operators

```javascript
// Spread: Expand iterable into individual elements

// Arrays
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]
const copy = [...arr1]; // Shallow copy

// Objects (ES2018)
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }
const merged = { ...defaults, ...userConfig };

// Function calls
Math.max(...[1, 2, 3]); // 3

// Rest: Collect remaining elements

// Function parameters
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10

// Destructuring
const [first, ...others] = [1, 2, 3, 4];
// first = 1, others = [2, 3, 4]

const { id, ...rest } = { id: 1, name: "Alice", age: 25 };
// id = 1, rest = { name: "Alice", age: 25 }
```

### 5.6 Modules

```javascript
// Named exports
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export class Calculator { /* ... */ }

// Named imports
import { PI, add } from './math.js';
import { add as sum } from './math.js'; // Rename
import * as Math from './math.js'; // Import all

// Default export
// logger.js
export default class Logger { /* ... */ }

// Default import
import Logger from './logger.js';
import MyLogger from './logger.js'; // Can use any name

// Mixed
// utils.js
export default function main() { }
export const helper1 = () => { };
export const helper2 = () => { };

// Import
import main, { helper1, helper2 } from './utils.js';

// Re-export
export { default as Logger } from './logger.js';
export * from './math.js';
```

### 5.7 Classes

```javascript
class Animal {
  // Class fields (ES2022)
  species = "Unknown";
  #privateField = "secret"; // Private field

  constructor(name) {
    this.name = name;
  }

  // Instance method
  speak() {
    console.log(`${this.name} makes a sound`);
  }

  // Getter/Setter
  get displayName() {
    return `Animal: ${this.name}`;
  }

  set displayName(value) {
    this.name = value;
  }

  // Static method
  static createAnonymous() {
    return new Animal("Anonymous");
  }

  // Private method (ES2022)
  #privateMethod() {
    return this.#privateField;
  }
}

// Inheritance
class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks`);
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // "Buddy barks"
```

### 5.8 Iterators & Generators

```javascript
// Iterable protocol
const iterable = {
  [Symbol.iterator]() {
    let i = 0;
    return {
      next() {
        if (i < 3) {
          return { value: i++, done: false };
        }
        return { done: true };
      },
    };
  },
};

for (const val of iterable) {
  console.log(val); // 0, 1, 2
}

// Generators - simpler way to create iterators
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }

// Infinite generator
function* infiniteSequence() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

// Generator delegation
function* combined() {
  yield* [1, 2];
  yield* [3, 4];
}

// Async generators (ES2018)
async function* fetchPages(urls) {
  for (const url of urls) {
    yield await fetch(url);
  }
}

for await (const response of fetchPages(urls)) {
  console.log(await response.text());
}
```

### 5.9 New Data Structures

```javascript
// Map - Key-value pairs (any type as key)
const map = new Map();
map.set("string", "value1");
map.set(123, "value2");
map.set({ key: 1 }, "value3");

map.get("string"); // "value1"
map.has(123); // true
map.size; // 3
map.delete("string");

for (const [key, value] of map) {
  console.log(key, value);
}

// Set - Unique values
const set = new Set([1, 2, 3, 3, 3]);
set.size; // 3
set.add(4);
set.has(2); // true
set.delete(1);

// Unique array
const unique = [...new Set([1, 2, 2, 3, 3, 3])]; // [1, 2, 3]

// WeakMap & WeakSet - Allow garbage collection
const weakMap = new WeakMap();
let obj = { data: "important" };
weakMap.set(obj, "metadata");
obj = null; // Object can be garbage collected

// Symbol - Unique identifier
const sym1 = Symbol("description");
const sym2 = Symbol("description");
sym1 === sym2; // false

// Well-known symbols
const obj = {
  [Symbol.toStringTag]: "CustomObject",
};
obj.toString(); // "[object CustomObject]"
```

---

## 6. Practice Exercises

### Exercise 1: Scope & Closures

```javascript
// 1.1: Predict the output
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 100);
}

// 1.2: Fix the code above to print 0, 1, 2

// 1.3: Create a counter module with private state
function createCounter() {
  // Implement: increment, decrement, getCount, reset
}

// 1.4: Implement a memoization function
function memoize(fn) {
  // Implement caching logic using closure
}
```

### Exercise 2: `this` Binding

```javascript
// 2.1: Predict output and explain
var obj = {
  name: "Object",
  greet: function () {
    return function () {
      console.log(this.name);
    };
  },
};
obj.greet()();

// 2.2: Fix it to print "Object"

// 2.3: Implement bind polyfill
Function.prototype.myBind = function (context) {
  // Implement
};

// 2.4: Predict output
function foo() {
  console.log(this.a);
}

var a = 2;
var obj = { a: 3, foo: foo };
var bar = obj.foo;

foo();
obj.foo();
bar();
```

### Exercise 3: Async Programming

```javascript
// 3.1: Predict the order of output
console.log(1);
setTimeout(() => console.log(2), 0);
Promise.resolve().then(() => console.log(3));
console.log(4);

// 3.2: Implement Promise.all
function promiseAll(promises) {
  // Implement
}

// 3.3: Convert callback to Promise
function readFileCallback(path, callback) {
  // Simulated async read
  setTimeout(() => {
    if (path) callback(null, "file content");
    else callback(new Error("No path"));
  }, 100);
}

function readFilePromise(path) {
  // Convert to Promise
}

// 3.4: Implement retry with exponential backoff
async function fetchWithRetry(url, maxRetries = 3) {
  // Implement
}
```

### Exercise 4: ES6+ Features

```javascript
// 4.1: Refactor using destructuring
function processUser(user) {
  const name = user.name;
  const age = user.age;
  const address = user.address;
  const city = address.city;
  // ...
}

// 4.2: Implement using class syntax
// Create Shape base class, Circle and Rectangle extending it
// Include area calculation methods

// 4.3: Create custom iterable
// Implement Range class that can be used in for...of
// new Range(1, 5) should iterate 1, 2, 3, 4, 5

// 4.4: Module exercise
// Create a module system for a simple todo app
// exports: addTodo, removeTodo, getTodos, toggleTodo
```

---

## 7. References

- [You Don't Know JS (1st Edition)](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed)
- [You Don't Know JS (2nd Edition)](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed)
- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [ECMAScript Specifications](https://tc39.es/ecma262/)

---

## 8. Summary

### Key Points to Remember:

1. **Scope & Closures:**

   - JavaScript is a compiled language (compiles right before execution)
   - Lexical scope is determined at author-time
   - Closure = function + its lexical scope
   - let/const create block scope, var creates function scope

2. **`this`:**

   - `this` is determined by call-site, NOT definition
   - 4 rules: Default → Implicit → Explicit → new
   - Arrow functions inherit `this` from enclosing scope

3. **Types & Coercion:**

   - 7 primitive types + object
   - `==` allows coercion, `===` doesn't
   - Only 6 falsy values

4. **Async:**

   - Event Loop: Call Stack, Web APIs, Callback Queue
   - Promises solve callback hell and inversion of control
   - async/await is syntactic sugar over Promises

5. **ES6+:**
   - let/const with block scope
   - Arrow functions with lexical this
   - Destructuring, spread/rest operators
   - Classes, Modules, Iterators/Generators

**Final Advice:** Don't just learn the syntax - understand the **mechanisms** behind it. That's how you truly "know" JavaScript!

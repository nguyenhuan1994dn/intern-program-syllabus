# Module 3: JavaScript Deep Dive

## Mục tiêu module

Module này cung cấp kiến thức chuyên sâu về JavaScript, dựa trên nội dung từ serie sách nổi tiếng **"You Don't Know JS"** của Kyle Simpson. Module sẽ giúp bạn hiểu sâu hơn về cách JavaScript thực sự hoạt động bên trong.

---

## 1. Scope & Closures (Phạm vi & Bao đóng)

### 1.1 Scope là gì?

#### Khái niệm

**Scope** là tập hợp các quy tắc xác định nơi lưu trữ biến và cách tìm kiếm biến sau đó. Đây là một trong những khái niệm cơ bản nhất của hầu hết các ngôn ngữ lập trình.

**Câu hỏi chính:**

- Các biến "sống" ở đâu?
- Chương trình tìm chúng như thế nào khi cần?

#### Compiler Theory

Mặc dù JavaScript thường được phân loại là ngôn ngữ "dynamic" hoặc "interpreted", thực tế nó là một **ngôn ngữ compiled (biên dịch)**. JavaScript engine thực hiện các bước tương tự như compiler truyền thống:

1. **Tokenizing/Lexing:** Chia chuỗi ký tự thành các token có ý nghĩa

   ```
   var a = 2; → [var] [a] [=] [2] [;]
   ```

2. **Parsing:** Chuyển stream của tokens thành AST (Abstract Syntax Tree)

   ```
   VariableDeclaration
   ├── Identifier (a)
   └── AssignmentExpression
       └── NumericLiteral (2)
   ```

3. **Code-Generation:** Chuyển AST thành mã thực thi

```javascript
// JavaScript KHÔNG chạy code trực tiếp như nhiều người nghĩ
// Mà compile trước (trong vài microseconds) rồi mới chạy

// Ví dụ: Khi gặp code này
var a = 2;

// Engine thực hiện:
// 1. Compiler hỏi Scope: "Đã có biến 'a' chưa?"
// 2. Nếu chưa → Compiler bảo Scope tạo biến mới
// 3. Sau đó Engine chạy: a = 2 (gán giá trị)
```

### 1.2 Lexical Scope

#### Khái niệm

**Lexical Scope** là scope được định nghĩa tại thời điểm viết code (author-time), dựa trên vị trí các hàm và block được đặt.

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
// Engine tìm kiếm biến từ trong ra ngoài
function outer() {
  var x = 10;

  function inner() {
    var y = 20;
    console.log(x + y); // x được tìm thấy ở outer scope
  }

  inner();
}

outer(); // 30
```

### 1.3 Function vs Block Scope

#### Function Scope

```javascript
// Mỗi function tạo một scope mới
function doSomething(a) {
  var b = a + 1; // b chỉ tồn tại trong function này

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
// let và const tạo block scope
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
    console.log(i); // 3, 3, 3 (vì var là function scoped)
  }, 100);
}

for (let j = 0; j < 3; j++) {
  setTimeout(function () {
    console.log(j); // 0, 1, 2 (vì let là block scoped)
  }, 100);
}
```

### 1.4 Hoisting

#### Khái niệm

**Hoisting** là hành vi mà các khai báo biến và hàm được "di chuyển" lên đầu scope trong phase compilation.

```javascript
// Code bạn viết:
console.log(a); // undefined (không phải ReferenceError!)
var a = 2;

// JavaScript "thấy" như thế này:
var a; // Declaration được hoisted
console.log(a); // undefined
a = 2; // Assignment ở nguyên chỗ

// Function declarations được hoisted toàn bộ
foo(); // "Hello!" - Hoạt động!

function foo() {
  console.log("Hello!");
}

// Function expressions KHÔNG được hoisted
bar(); // ❌ TypeError: bar is not a function

var bar = function () {
  console.log("World!");
};

// let/const cũng hoisting nhưng ở "Temporal Dead Zone"
// console.log(x); // ❌ ReferenceError: Cannot access 'x' before initialization
let x = 10;
```

#### Thứ tự ưu tiên

```javascript
foo(); // 1

var foo;

function foo() {
  console.log(1);
}

foo = function () {
  console.log(2);
};

// Functions được hoisted TRƯỚC variables
// Duplicate function declarations → sau override trước

function foo() {
  console.log(1);
}

function foo() {
  console.log(3); // Function này override function trước
}

foo(); // 3
```

### 1.5 Closures (Bao đóng)

#### Khái niệm

> **Closure là khi một function có thể nhớ và truy cập lexical scope của nó ngay cả khi function đó đang thực thi bên ngoài lexical scope đó.**

```javascript
function foo() {
  var a = 2;

  function bar() {
    console.log(a);
  }

  return bar;
}

var baz = foo();

baz(); // 2 -- Đây là closure!

/*
Giải thích:
1. foo() thực thi xong, thường thì garbage collector sẽ dọn dẹp scope của foo
2. Nhưng bar() vẫn giữ reference đến scope đó
3. Vì vậy scope không bị xóa, bar() vẫn truy cập được 'a'
4. Reference này gọi là CLOSURE
*/
```

#### Thực tế: Closure ở khắp nơi

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
// Tất cả closure đều share cùng 1 'i', và khi setTimeout chạy, i = 6

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
// let tạo binding mới cho mỗi iteration
```

---

## 2. this & Object Prototypes

### 2.1 `this` là gì?

#### Khái niệm

`this` là một binding được tạo cho mỗi function invocation, và giá trị của nó phụ thuộc hoàn toàn vào **call-site** (nơi function được gọi).

```javascript
// ❌ Hiểu sai thường gặp #1: this trỏ đến chính function
function foo(num) {
  console.log("foo: " + num);
  this.count++; // KHÔNG trỏ đến foo.count!
}

foo.count = 0;

for (var i = 0; i < 10; i++) {
  foo(i);
}

console.log(foo.count); // 0 - vẫn là 0!

// ❌ Hiểu sai thường gặp #2: this trỏ đến scope của function
// → KHÔNG BAO GIỜ đúng trong JavaScript
```

### 2.2 Bốn quy tắc xác định `this`

#### Call-site & Call-stack

```javascript
function baz() {
  // call-stack: `baz`
  // call-site: global scope

  console.log("baz");
  bar(); // ← call-site cho `bar`
}

function bar() {
  // call-stack: `baz` → `bar`
  // call-site: trong `baz`

  console.log("bar");
  foo(); // ← call-site cho `foo`
}

function foo() {
  // call-stack: `baz` → `bar` → `foo`
  // call-site: trong `bar`

  console.log("foo");
}

baz(); // ← call-site cho `baz`
```

#### Rule 1: Default Binding

```javascript
// Khi function được gọi "plain" - không có context
function foo() {
  console.log(this.a);
}

var a = 2;

foo(); // 2 (trong non-strict mode)
// this trỏ đến global object (window trong browser)

// Trong strict mode:
function bar() {
  "use strict";
  console.log(this.a);
}

bar(); // TypeError: Cannot read property 'a' of undefined
```

#### Rule 2: Implicit Binding

```javascript
// Khi function được gọi với context object
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

obj.foo(); // 2 - this = obj

// Chỉ level cuối cùng mới quan trọng
var obj1 = {
  a: 2,
  obj2: {
    a: 42,
    foo: foo,
  },
};

obj1.obj2.foo(); // 42 - this = obj2

// ⚠️ Implicitly Lost - Bẫy phổ biến!
var bar = obj.foo; // function reference
var a = "oops, global";

bar(); // "oops, global" - Mất implicit binding!

// ⚠️ Callback cũng mất binding
function doFoo(fn) {
  fn(); // ← call-site là plain call
}

doFoo(obj.foo); // "oops, global"

// setTimeout cũng vậy
setTimeout(obj.foo, 100); // "oops, global"
```

#### Rule 3: Explicit Binding

```javascript
// Sử dụng call(), apply(), hoặc bind()
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
};

foo.call(obj); // 2 - explicitly bind this = obj
foo.apply(obj); // 2 - tương tự call

// Khác biệt call vs apply:
function greet(greeting, punctuation) {
  console.log(greeting + ", " + this.name + punctuation);
}

var person = { name: "Alice" };

greet.call(person, "Hello", "!"); // "Hello, Alice!"
greet.apply(person, ["Hi", "?"]); // "Hi, Alice?"

// Hard Binding với bind()
var bar = foo.bind(obj);

bar(); // 2
setTimeout(bar, 100); // 2 - không bị mất binding
bar.call(window); // 2 - không thể override
```

#### Rule 4: `new` Binding

```javascript
// Khi function được gọi với 'new' keyword
function Foo(a) {
  this.a = a;
}

var bar = new Foo(2);
console.log(bar.a); // 2

// Khi gọi new:
// 1. Object mới được tạo
// 2. Object được link đến prototype
// 3. Object được set làm 'this' cho function call
// 4. Trả về object đó (trừ khi function return object khác)
```

#### Thứ tự ưu tiên

```javascript
// new > explicit (call/apply/bind) > implicit > default

// 1. Function được gọi với new?
//    → this = object mới được tạo

// 2. Function được gọi với call/apply/bind?
//    → this = object được chỉ định

// 3. Function được gọi với context object?
//    → this = context object đó

// 4. Default:
//    → strict mode: this = undefined
//    → non-strict: this = global object

// Ví dụ thực tế
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
console.log(obj1.a); // 2 (không đổi)
console.log(bar.a); // 4
```

### 2.3 Arrow Functions và Lexical `this`

```javascript
// Arrow functions KHÔNG có this riêng
// Chúng inherit this từ enclosing scope (lexical this)

function foo() {
  // Arrow function capture 'this' của foo
  return (a) => {
    console.log(this.a);
  };
}

var obj1 = { a: 2 };
var obj2 = { a: 3 };

var bar = foo.call(obj1);
bar.call(obj2); // 2, không phải 3!
// Arrow function giữ this = obj1, không thể override

// Trước ES6 - phải dùng self = this
function foo() {
  var self = this;
  setTimeout(function () {
    console.log(self.a); // sử dụng closure
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
// Mọi object đều có internal property [[Prototype]]
// Đây là reference đến object khác

var anotherObject = {
  a: 2,
};

// Tạo object được linked đến anotherObject
var myObject = Object.create(anotherObject);

myObject.a; // 2 - tìm thấy qua prototype chain

/*
myObject
  [[Prototype]] → anotherObject
                    a: 2
                    [[Prototype]] → Object.prototype
                                      [[Prototype]] → null
*/

// for..in và 'in' operator cũng check prototype chain
for (var k in myObject) {
  console.log("found: " + k); // "found: a"
}

"a" in myObject; // true
```

#### "Class" Pattern trong JavaScript

```javascript
// JavaScript KHÔNG có classes theo nghĩa truyền thống
// Chỉ có objects linked đến objects

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

// Tạo Bar.prototype linked đến Foo.prototype
Bar.prototype = Object.create(Foo.prototype);

// Thêm method riêng
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
// Objects Linked to Other Objects - Không cần "class"

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

## 3. Types & Coercion (Kiểu dữ liệu & Ép kiểu)

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
typeof null === "object"; // true - BUG lịch sử!
typeof function a() {} === "function"; // true - subtype của object

// Kiểm tra null
var a = null;
!a && typeof a === "object"; // true

// Arrays cũng là objects
typeof [1, 2, 3] === "object"; // true
Array.isArray([1, 2, 3]); // true
```

### 3.2 Values vs References

```javascript
// Primitives: passed by value
var a = 2;
var b = a; // copy of value
b++;
a; // 2 - không đổi

// Objects: passed by reference
var c = [1, 2, 3];
var d = c; // copy of reference
d.push(4);
c; // [1, 2, 3, 4] - cùng array!

// ⚠️ Re-assignment không thay đổi reference ban đầu
function foo(x) {
  x.push(4);
  x; // [1, 2, 3, 4]

  x = [4, 5, 6]; // tạo reference MỚI
  x.push(7);
  x; // [4, 5, 6, 7]
}

var a = [1, 2, 3];
foo(a);
a; // [1, 2, 3, 4] - không phải [4, 5, 6, 7]
```

### 3.3 Coercion (Ép kiểu)

#### ToBoolean - Falsy vs Truthy

```javascript
// ⚠️ Chỉ có ĐÚNG 6 giá trị falsy:
// • undefined
// • null
// • false
// • +0, -0, NaN
// • "" (empty string)

// MỌI THỨ KHÁC đều truthy!
Boolean(undefined); // false
Boolean(null); // false
Boolean(0); // false
Boolean(""); // false
Boolean(NaN); // false

Boolean("0"); // true ← String không rỗng!
Boolean([]); // true ← Empty array là truthy!
Boolean({}); // true ← Empty object là truthy!
Boolean(function () {}); // true

// ⚠️ Bẫy thường gặp
if ([]) {
  console.log("Empty array is truthy!"); // Chạy!
}

// Nhưng...
[] == false; // true! (coercion phức tạp)
```

#### Explicit vs Implicit Coercion

```javascript
// EXPLICIT coercion - rõ ràng, dễ đọc
var a = 42;
var b = String(a); // "42"
var c = a.toString(); // "42"

var d = "3.14";
var e = Number(d); // 3.14
var f = +d; // 3.14 (unary + operator)
var g = parseInt(d); // 3 (chỉ integer)

// IMPLICIT coercion - xảy ra ngầm
var a = "42";
var b = a * 1; // 42 - string → number

var c = 42;
var d = c + ""; // "42" - number → string

// Boolean coercion
var a = 42;
var b = !!a; // true - explicit
if (a) {
} // implicit trong condition
```

#### `==` vs `===`

```javascript
// ⚠️ HIỂU SAI phổ biến:
// "== kiểm tra value, === kiểm tra value VÀ type"

// ✅ ĐÚNG:
// "== cho phép coercion, === không cho phép coercion"

// Nếu cùng type → cả hai hoạt động giống nhau
42 === 42; // true
42 == 42; // true

"42" === "42"; // true
"42" == "42"; // true

// Khác type → == sẽ coerce
"42" == 42; // true (string → number)
"42" === 42; // false

// ⚠️ Edge cases cần nhớ
null == undefined; // true (đặc biệt)
null === undefined; // false

NaN == NaN; // false (NaN không bằng chính nó!)
NaN === NaN; // false

// Khuyến nghị:
// • Dùng === khi không chắc về types
// • Dùng == khi biết rõ types và muốn coercion
// • LUÔN dùng === với null/undefined
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

#### Khái niệm

JavaScript là single-threaded, nhưng có thể xử lý async operations thông qua **Event Loop**.

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
// Promises sử dụng Job Queue (microtasks)
// Microtasks được ưu tiên hơn macrotasks (setTimeout, setInterval)

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
// Khi truyền callback, bạn KHÔNG kiểm soát:
// • Callback được gọi đúng lúc không?
// • Callback được gọi đúng số lần không?
// • Callback có được truyền đúng arguments không?
// • Errors có được handle không?

// Example: Third-party analytics
analytics.track(paymentData, function () {
  chargeCreditCard(); // Bạn tin tưởng họ gọi đúng 1 lần?
});
```

### 4.3 Promises

#### Khái niệm

**Promise** là một placeholder cho future value - một cơ chế để xử lý async operations với nhiều guarantees hơn callbacks.

```javascript
// Promise states:
// • Pending: đang chờ
// • Fulfilled: thành công với value
// • Rejected: thất bại với reason

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
// Mỗi then() trả về Promise MỚI
// Giá trị return từ callback = fulfillment value của Promise mới

Promise.resolve(21)
  .then(function (v) {
    console.log(v); // 21
    return v * 2; // fulfill với 42
  })
  .then(function (v) {
    console.log(v); // 42
    // return Promise để tạo async step
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
// Promise.all - Chờ TẤT CẢ hoàn thành
// Fail fast: reject ngay khi 1 Promise reject

Promise.all([fetchUser(1), fetchUser(2), fetchUser(3)])
  .then(function (users) {
    console.log(users); // [user1, user2, user3]
  })
  .catch(function (err) {
    console.error("At least one failed:", err);
  });

// Promise.race - Chờ Promise ĐẦU TIÊN settle
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

// Promise.allSettled (ES2020) - Chờ TẤT CẢ settle (fulfill hoặc reject)
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

// Promise.any (ES2021) - Chờ Promise ĐẦU TIÊN fulfill
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
// Syntactic sugar trên Promises
// Viết async code giống sync code

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

// Parallel execution với async/await
async function fetchAllUsers() {
  // ❌ Sequential - chậm
  const user1 = await fetchUser(1);
  const user2 = await fetchUser(2);
  const user3 = await fetchUser(3);

  // ✅ Parallel - nhanh
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

## 6. Bài tập thực hành

### Bài tập 1: Scope & Closures

```javascript
// 1.1: Dự đoán output
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 100);
}

// 1.2: Sửa code trên để in ra 0, 1, 2

// 1.3: Tạo counter module với private state
function createCounter() {
  // Implement: increment, decrement, getCount, reset
}

// 1.4: Implement memoization function
function memoize(fn) {
  // Implement caching logic using closure
}
```

### Bài tập 2: `this` Binding

```javascript
// 2.1: Dự đoán output và giải thích
var obj = {
  name: "Object",
  greet: function () {
    return function () {
      console.log(this.name);
    };
  },
};
obj.greet()();

// 2.2: Sửa lại để in ra "Object"

// 2.3: Implement bind polyfill
Function.prototype.myBind = function (context) {
  // Implement
};

// 2.4: Dự đoán output
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

### Bài tập 3: Async Programming

```javascript
// 3.1: Dự đoán thứ tự output
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

### Bài tập 4: ES6+ Features

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

## 7. Tài liệu tham khảo

- [You Don't Know JS (1st Edition)](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed)
- [You Don't Know JS (2nd Edition)](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed)
- [MDN Web Docs - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [ECMAScript Specifications](https://tc39.es/ecma262/)

---

## 8. Tổng kết

### Những điểm quan trọng cần nhớ:

1. **Scope & Closures:**

   - JavaScript là compiled language (compile ngay trước khi chạy)
   - Lexical scope được xác định tại author-time
   - Closure = function + lexical scope của nó
   - let/const tạo block scope, var tạo function scope

2. **`this`:**

   - `this` được xác định bởi call-site, KHÔNG phải định nghĩa
   - 4 rules: Default → Implicit → Explicit → new
   - Arrow functions inherit `this` từ enclosing scope

3. **Types & Coercion:**

   - 7 primitive types + object
   - `==` cho phép coercion, `===` không
   - Chỉ có 6 falsy values

4. **Async:**

   - Event Loop: Call Stack, Web APIs, Callback Queue
   - Promises giải quyết callback hell và inversion of control
   - async/await là syntactic sugar trên Promises

5. **ES6+:**
   - let/const với block scope
   - Arrow functions với lexical this
   - Destructuring, spread/rest operators
   - Classes, Modules, Iterators/Generators

**Lời khuyên cuối cùng:** Đừng chỉ học syntax - hãy hiểu **cơ chế** đằng sau. Đó là cách để thực sự "know" JavaScript!

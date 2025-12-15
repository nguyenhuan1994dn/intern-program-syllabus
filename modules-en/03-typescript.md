# Module 3: TypeScript

## Module Objectives

This module helps you understand and use TypeScript - một superset của JavaScript với type system mạnh mẽ, giúp code an toàn và dễ maintain hơn.

---

## 1. TypeScript là gì và cách hoạt động

### Concept

**TypeScript** là ngôn ngữ lập trình được phát triển bởi Microsoft, mở rộng JavaScript bằng cách thêm:

- **Static Type Checking**: Kiểm tra kiểu dữ liệu lúc compile-time
- **Type Annotations**: Khai báo kiểu dữ liệu rõ ràng
- **Advanced Features**: Interfaces, Generics, Decorators, etc.

**Cách hoạt động:**

```
TypeScript Code (.ts) → TypeScript Compiler (tsc) → JavaScript Code (.js) → Browser/Node.js
```

**Lợi ích:**

- ✅ Phát hiện lỗi sớm (compile-time thay vì runtime)
- ✅ IntelliSense tốt hơn (autocomplete, type hints)
- ✅ Refactoring an toàn hơn
- ✅ Code dễ đọc và maintain

### Examples

```typescript
// JavaScript (no type checking)
function add(a, b) {
  return a + b;
}
console.log(add(5, "10")); // "510" - Bug nhưng không báo lỗi!

// TypeScript (với type checking)
function addTS(a: number, b: number): number {
  return a + b;
}
// console.log(addTS(5, "10")); // ❌ Compile error!
console.log(addTS(5, 10)); // ✅ 15

// Setup TypeScript project
/*
1. Install TypeScript
npm install -g typescript

2. Initialize tsconfig.json
tsc --init

3. Compile TypeScript file
tsc app.ts

4. Watch mode
tsc --watch
*/
```

**tsconfig.json cơ bản:**

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

---

## 2. Tại sao TypeScript có lợi thế hơn vanilla JavaScript

### Concept

TypeScript giải quyết các vấn đề của JavaScript:

**1. Type Safety:**

```typescript
// JavaScript - Runtime error
function getUserName(user) {
  return user.name.toUpperCase(); // Crash nếu user là null!
}

// TypeScript - Compile time error
function getUserNameTS(user: { name: string } | null): string {
  // return user.name.toUpperCase(); // ❌ Error: Object is possibly 'null'
  return user ? user.name.toUpperCase() : "Unknown";
}
```

**2. Better IDE Support:**

- Autocomplete chính xác
- Type hints
- Inline documentation
- Refactoring tools

**3. Self-documenting Code:**

```typescript
// TypeScript - Types serve as documentation
interface User {
  id: number;
  name: string;
  email: string;
  role: "admin" | "user" | "guest";
  createdAt: Date;
}

function createUser(data: User): Promise<User> {
  // Implementation
}

// Ai đọc code đều biết structure của User mà không cần docs!
```

**4. Catch Bugs Early:**

```typescript
// JavaScript - Bug sẽ xuất hiện ở production
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

users.forEach((user) => {
  console.log(user.nmae); // Typo! undefined
});

// TypeScript - Phát hiện ngay
interface User {
  id: number;
  name: string;
}

const usersTS: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

usersTS.forEach((user) => {
  // console.log(user.nmae); // ❌ Error: Property 'nmae' does not exist
  console.log(user.name); // ✅ Correct
});
```

### Examples so sánh

```typescript
// ========== JAVASCRIPT ==========
// Bug chỉ xuất hiện khi runtime
function calculateDiscount(price, discount) {
  return price - (price * discount) / 100;
}

calculateDiscount(100, "20"); // NaN - Bug!
calculateDiscount("100", 20); // "100..." - Bug!

// ========== TYPESCRIPT ==========
// Bug bị phát hiện ngay khi viết code
function calculateDiscountTS(price: number, discount: number): number {
  return price - (price * discount) / 100;
}

// calculateDiscountTS(100, "20"); // ❌ Compile error
// calculateDiscountTS("100", 20); // ❌ Compile error
calculateDiscountTS(100, 20); // ✅ 80

// ========== COMPLEX EXAMPLE ==========
// JavaScript
function processOrder(order) {
  const total = order.items.reduce((sum, item) => {
    return sum + item.price * item.quantity;
  }, 0);

  if (order.discount) {
    total = total - (total * order.discount) / 100;
  }

  return {
    orderId: order.id,
    total: total,
    status: order.status,
  };
}

// TypeScript
interface OrderItem {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

interface Order {
  id: string;
  items: OrderItem[];
  discount?: number;
  status: "pending" | "processing" | "completed" | "cancelled";
}

interface ProcessedOrder {
  orderId: string;
  total: number;
  status: Order["status"];
}

function processOrderTS(order: Order): ProcessedOrder {
  let total = order.items.reduce((sum, item) => {
    return sum + item.price * item.quantity;
  }, 0);

  if (order.discount) {
    total = total - (total * order.discount) / 100;
  }

  return {
    orderId: order.id,
    total: total,
    status: order.status,
  };
}
```

---

## 3. TypeScript và các tính năng

### 3.1 Types (Các kiểu dữ liệu)

#### Concept

TypeScript cung cấp nhiều types:

- **Primitive Types**: number, string, boolean, null, undefined, symbol, bigint
- **Object Types**: object, array, tuple, function
- **Special Types**: any, unknown, void, never

#### Examples

```typescript
// Primitive Types
let age: number = 25;
let name: string = "Alice";
let isStudent: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;

// Array Types
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];
let mixed: (string | number)[] = [1, "two", 3];

// Tuple (array with fixed length and types)
let person: [string, number] = ["Alice", 25];
// person = [25, "Alice"]; // ❌ Error: wrong order
// person = ["Alice"];     // ❌ Error: missing element

// Object Type
let user: { name: string; age: number } = {
  name: "Alice",
  age: 25,
};

// Function Type
let add: (a: number, b: number) => number;
add = (x, y) => x + y;

// any (avoid using!)
let anything: any = "hello";
anything = 42;
anything = true;
anything.foo.bar; // No error, but dangerous!

// unknown (safer than any)
let value: unknown = "hello";
// let str: string = value; // ❌ Error
if (typeof value === "string") {
  let str: string = value; // ✅ OK after type check
}

// void (for functions that don't return)
function logMessage(message: string): void {
  console.log(message);
  // No return value
}

// never (for functions that never return)
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### 3.2 ES6 Support

#### Concept

TypeScript hỗ trợ tất cả tính năng ES6+ và compile về ES5 nếu cần.

#### Examples

```typescript
// Arrow Functions
const greet = (name: string): string => `Hello, ${name}!`;

// Destructuring
const user = { name: "Alice", age: 25, email: "alice@example.com" };
const { name, age }: { name: string; age: number } = user;

const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest]: number[] = numbers;

// Template Literals
const message: string = `User ${name} is ${age} years old`;

// Default Parameters
function createUser(name: string, age: number = 18): User {
  return { name, age };
}

// Rest Parameters
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

// Spread Operator
const arr1: number[] = [1, 2, 3];
const arr2: number[] = [...arr1, 4, 5];

const user1 = { name: "Alice", age: 25 };
const user2 = { ...user1, email: "alice@example.com" };

// Classes
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hello, I'm ${this.name}`;
  }
}

// Modules
export const PI = 3.14159;
export function calculateArea(radius: number): number {
  return PI * radius * radius;
}

// Promises
function fetchUser(id: number): Promise<User> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ id, name: "Alice", age: 25 });
    }, 1000);
  });
}

// Async/Await
async function getUser(id: number): Promise<User> {
  const user = await fetchUser(id);
  return user;
}
```

### 3.3 Classes

#### Concept

TypeScript classes với access modifiers, abstract classes, và static members.

#### Examples

```typescript
// Basic Class
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound(): void {
    console.log("Some generic sound");
  }
}

// Inheritance
class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name);
    this.breed = breed;
  }

  makeSound(): void {
    console.log("Woof! Woof!");
  }

  fetch(): void {
    console.log(`${this.name} is fetching the ball`);
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.makeSound(); // "Woof! Woof!"

// Access Modifiers
class BankAccount {
  public accountNumber: string; // Accessible everywhere
  private balance: number; // Only in this class
  protected ownerName: string; // This class and subclasses

  constructor(
    accountNumber: string,
    ownerName: string,
    initialBalance: number
  ) {
    this.accountNumber = accountNumber;
    this.ownerName = ownerName;
    this.balance = initialBalance;
  }

  public deposit(amount: number): void {
    this.balance += amount;
  }

  public withdraw(amount: number): boolean {
    if (amount <= this.balance) {
      this.balance -= amount;
      return true;
    }
    return false;
  }

  public getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount("123456", "Alice", 1000);
console.log(account.accountNumber); // ✅ OK
// console.log(account.balance);    // ❌ Error: private
account.deposit(500);
console.log(account.getBalance()); // 1500

// Shorthand Constructor
class User {
  constructor(
    public id: number,
    public name: string,
    private password: string
  ) {}

  verifyPassword(password: string): boolean {
    return this.password === password;
  }
}

// Static Members
class MathUtils {
  static PI: number = 3.14159;

  static calculateCircleArea(radius: number): number {
    return this.PI * radius * radius;
  }
}

console.log(MathUtils.PI);
console.log(MathUtils.calculateCircleArea(5));

// Abstract Classes
abstract class Shape {
  constructor(public color: string) {}

  abstract calculateArea(): number;

  describe(): void {
    console.log(`A ${this.color} shape with area ${this.calculateArea()}`);
  }
}

class Circle extends Shape {
  constructor(color: string, public radius: number) {
    super(color);
  }

  calculateArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle = new Circle("red", 5);
circle.describe();

// Readonly Properties
class Point {
  readonly x: number;
  readonly y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const point = new Point(10, 20);
// point.x = 30; // ❌ Error: readonly property
```

### 3.4 Modules

#### Concept

TypeScript hỗ trợ ES6 modules với type safety.

#### Examples

```typescript
// ========== math.ts ==========
export const PI = 3.14159;

export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export class Calculator {
  multiply(a: number, b: number): number {
    return a * b;
  }

  divide(a: number, b: number): number {
    if (b === 0) throw new Error("Division by zero");
    return a / b;
  }
}

// Default export
export default function square(x: number): number {
  return x * x;
}

// ========== types.ts ==========
export interface User {
  id: number;
  name: string;
  email: string;
}

export type Role = "admin" | "user" | "guest";

export enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING",
}

// ========== app.ts ==========
// Named imports
import { PI, add, Calculator } from "./math";
import { User, Role, Status } from "./types";

// Default import
import square from "./math";

// Import everything
import * as MathUtils from "./math";

// Using imports
console.log(PI);
console.log(add(5, 3));

const calc = new Calculator();
console.log(calc.multiply(4, 5));

console.log(square(5));

console.log(MathUtils.PI);
console.log(MathUtils.add(10, 20));

// Type-only imports (TypeScript 3.8+)
import type { User } from "./types";

// Re-exporting
export { User, Role } from "./types";
export * from "./math";
```

### 3.5 Interfaces

#### Concept

Interface định nghĩa contract cho objects, classes, và functions.

#### Examples

```typescript
// Basic Interface
interface User {
  id: number;
  name: string;
  email: string;
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

// Optional Properties
interface Product {
  id: number;
  name: string;
  price: number;
  description?: string; // Optional
}

const product: Product = {
  id: 1,
  name: "Laptop",
  price: 999,
  // description is optional
};

// Readonly Properties
interface Point {
  readonly x: number;
  readonly y: number;
}

const point: Point = { x: 10, y: 20 };
// point.x = 30; // ❌ Error: readonly

// Function Interface
interface MathFunction {
  (a: number, b: number): number;
}

const add: MathFunction = (x, y) => x + y;
const subtract: MathFunction = (x, y) => x - y;

// Interface for Class
interface Animal {
  name: string;
  makeSound(): void;
}

class Dog implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound(): void {
    console.log("Woof!");
  }
}

// Extending Interfaces
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  employeeId: string;
  department: string;
}

const employee: Employee = {
  name: "Alice",
  age: 30,
  employeeId: "E001",
  department: "Engineering",
};

// Multiple Interface Inheritance
interface Printable {
  print(): void;
}

interface Loggable {
  log(): void;
}

interface Document extends Printable, Loggable {
  title: string;
}

class Report implements Document {
  title: string;

  constructor(title: string) {
    this.title = title;
  }

  print(): void {
    console.log(`Printing: ${this.title}`);
  }

  log(): void {
    console.log(`Logging: ${this.title}`);
  }
}

// Index Signatures
interface StringDictionary {
  [key: string]: string;
}

const dict: StringDictionary = {
  name: "Alice",
  role: "Developer",
};

interface NumberArray {
  [index: number]: number;
}

const arr: NumberArray = [1, 2, 3, 4, 5];
```

---

## 4. Khác nhau của TS vs JS

### Concept

Những điểm khác biệt chính giữa TypeScript và JavaScript.

### Comparison

| Feature              | JavaScript        | TypeScript            |
| -------------------- | ----------------- | --------------------- |
| **Typing**           | Dynamic (runtime) | Static (compile-time) |
| **Type Annotations** | ❌ No             | ✅ Yes                |
| **Interfaces**       | ❌ No             | ✅ Yes                |
| **Generics**         | ❌ No             | ✅ Yes                |
| **Enums**            | ❌ No             | ✅ Yes                |
| **Compile**          | No compilation    | Compiles to JS        |
| **IDE Support**      | Basic             | Advanced              |
| **Learning Curve**   | Easier            | Steeper               |
| **File Extension**   | .js               | .ts, .tsx             |
| **Error Detection**  | Runtime           | Compile-time          |

### Examples

```typescript
// ========== JAVASCRIPT ==========
// 1. No type checking
function multiply(a, b) {
  return a * b;
}
multiply(5, "10"); // "5050" - Wrong!

// 2. No interfaces
const user = {
  name: "Alice",
  age: 25,
};
// No contract, no validation

// 3. No enums
const STATUS_ACTIVE = "ACTIVE";
const STATUS_INACTIVE = "INACTIVE";

// 4. No generics
function getFirst(arr) {
  return arr[0];
}

// ========== TYPESCRIPT ==========
// 1. Type checking
function multiplyTS(a: number, b: number): number {
  return a * b;
}
// multiplyTS(5, "10"); // ❌ Compile error!

// 2. Interfaces
interface UserTS {
  name: string;
  age: number;
}

const userTS: UserTS = {
  name: "Alice",
  age: 25,
};

// 3. Enums
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
}

let status: Status = Status.Active;

// 4. Generics
function getFirstTS<T>(arr: T[]): T {
  return arr[0];
}

const firstNum = getFirstTS<number>([1, 2, 3]); // number
const firstStr = getFirstTS<string>(["a", "b", "c"]); // string

// ========== ADVANCED COMPARISON ==========
// JavaScript - Runtime errors
class BankAccountJS {
  constructor(balance) {
    this.balance = balance;
  }

  withdraw(amount) {
    this.balance -= amount; // No validation!
  }
}

const account = new BankAccountJS(100);
account.withdraw("50"); // balance becomes "10050" - Bug!

// TypeScript - Compile-time safety
class BankAccountTS {
  private balance: number;

  constructor(balance: number) {
    this.balance = balance;
  }

  withdraw(amount: number): void {
    if (amount > this.balance) {
      throw new Error("Insufficient funds");
    }
    this.balance -= amount;
  }

  getBalance(): number {
    return this.balance;
  }
}

const accountTS = new BankAccountTS(100);
// accountTS.withdraw("50"); // ❌ Compile error!
accountTS.withdraw(50); // ✅ OK
```

---

## 5. Các kiểu dữ liệu trong TypeScript

### 5.1 Interface

_(Đã đề cập ở phần 3.5)_

### 5.2 Type

#### Concept

`type` alias tạo tên mới cho một type. Tương tự interface nhưng linh hoạt hơn.

#### Examples

```typescript
// Type Alias
type ID = string | number;
type User = {
  id: ID;
  name: string;
  email: string;
};

const user: User = {
  id: "abc123",
  name: "Alice",
  email: "alice@example.com",
};

// Union Types
type Status = "pending" | "approved" | "rejected";
let orderStatus: Status = "pending";
// orderStatus = "shipped"; // ❌ Error

// Intersection Types
type Person = {
  name: string;
  age: number;
};

type Employee = {
  employeeId: string;
  department: string;
};

type EmployeePerson = Person & Employee;

const emp: EmployeePerson = {
  name: "Alice",
  age: 30,
  employeeId: "E001",
  department: "IT",
};

// Function Types
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (x, y) => x + y;
const multiply: MathOperation = (x, y) => x * y;

// Tuple Types
type Point = [number, number];
type RGB = [number, number, number];

const point: Point = [10, 20];
const color: RGB = [255, 0, 0];

// Type vs Interface
// ✅ Type can use unions
type StringOrNumber = string | number;

// ✅ Type can use tuples
type Coordinate = [number, number];

// ✅ Interface can be extended
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

// ✅ Interface can be merged (declaration merging)
interface Window {
  title: string;
}

interface Window {
  width: number;
}

// Merged: { title: string; width: number; }
```

### 5.3 Enum

#### Concept

Enum định nghĩa tập hợp các hằng số có tên.

#### Examples

```typescript
// Numeric Enum
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}

let dir: Direction = Direction.Up;
console.log(dir); // 0

// Custom Numeric Values
enum Status {
  Pending = 1,
  Approved = 2,
  Rejected = 3,
}

// String Enum
enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE",
}

let color: Color = Color.Red;
console.log(color); // "RED"

// Heterogeneous Enum (not recommended)
enum Mixed {
  No = 0,
  Yes = "YES",
}

// Const Enum (more performant)
const enum HttpStatus {
  OK = 200,
  BadRequest = 400,
  NotFound = 404,
  InternalServerError = 500,
}

let status: HttpStatus = HttpStatus.OK;

// Computed Enum
enum FileAccess {
  None = 0,
  Read = 1 << 0, // 1
  Write = 1 << 1, // 2
  ReadWrite = Read | Write, // 3
}

// Using Enums
function handleStatus(status: Status): string {
  switch (status) {
    case Status.Pending:
      return "Order is pending";
    case Status.Approved:
      return "Order approved";
    case Status.Rejected:
      return "Order rejected";
    default:
      return "Unknown status";
  }
}

console.log(handleStatus(Status.Approved));
```

---

## 6. Advanced TypeScript

### 6.1 Utility Types

#### Concept

TypeScript cung cấp built-in utility types để transform types.

#### Examples

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Partial<T> - Make all properties optional
type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; age?: number; }

function updateUser(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}

// Required<T> - Make all properties required
interface PartialConfig {
  apiUrl?: string;
  timeout?: number;
}

type RequiredConfig = Required<PartialConfig>;
// { apiUrl: string; timeout: number; }

// Readonly<T> - Make all properties readonly
type ReadonlyUser = Readonly<User>;
const user: ReadonlyUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  age: 25,
};
// user.name = "Bob"; // ❌ Error: readonly

// Pick<T, K> - Pick specific properties
type UserPreview = Pick<User, "id" | "name">;
// { id: number; name: string; }

// Omit<T, K> - Omit specific properties
type UserWithoutAge = Omit<User, "age">;
// { id: number; name: string; email: string; }

// Record<K, T> - Create object type with keys K and values T
type UserRoles = Record<string, "admin" | "user" | "guest">;
const roles: UserRoles = {
  alice: "admin",
  bob: "user",
  charlie: "guest",
};

// Exclude<T, U> - Exclude types from union
type T1 = Exclude<"a" | "b" | "c", "a">;
// "b" | "c"

// Extract<T, U> - Extract types from union
type T2 = Extract<"a" | "b" | "c", "a" | "f">;
// "a"

// NonNullable<T> - Remove null and undefined
type T3 = NonNullable<string | number | null | undefined>;
// string | number

// ReturnType<T> - Get return type of function
function getUser(): User {
  return { id: 1, name: "Alice", email: "alice@example.com", age: 25 };
}

type UserReturnType = ReturnType<typeof getUser>;
// User

// Parameters<T> - Get parameters type of function
function createUser(name: string, age: number): User {
  return { id: 1, name, email: "", age };
}

type CreateUserParams = Parameters<typeof createUser>;
// [string, number]
```

### 6.2 Keyof Type Operator

#### Concept

`keyof` creates a union of all property keys của một object type.

#### Examples

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKeys = keyof User;
// "id" | "name" | "email"

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

const name = getProperty(user, "name"); // string
const id = getProperty(user, "id"); // number
// const invalid = getProperty(user, "age"); // ❌ Error

// Mapped Types
type Optional<T> = {
  [K in keyof T]?: T[K];
};

type PartialUser = Optional<User>;
// { id?: number; name?: string; email?: string; }

type ReadonlyType<T> = {
  readonly [K in keyof T]: T[K];
};

type ReadonlyUser = ReadonlyType<User>;
```

### 6.3 Typeof Type Operator

#### Concept

`typeof` lấy type của một value.

#### Examples

```typescript
const user = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

type User = typeof user;
// { id: number; name: string; email: string; }

function add(a: number, b: number): number {
  return a + b;
}

type AddFunction = typeof add;
// (a: number, b: number) => number

const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  retries: 3,
} as const;

type Config = typeof config;
// {
//   readonly apiUrl: "https://api.example.com";
//   readonly timeout: 5000;
//   readonly retries: 3;
// }
```

### 6.4 Type Annotations

_(Đã đề cập trong các phần trước)_

### 6.5 Type Inference

#### Concept

TypeScript tự động suy luận types khi không được khai báo rõ ràng.

#### Examples

```typescript
// Type Inference
let x = 10; // inferred as number
let y = "hello"; // inferred as string
let z = true; // inferred as boolean

// Array inference
let numbers = [1, 2, 3]; // number[]
let mixed = [1, "two", 3]; // (string | number)[]

// Function return type inference
function add(a: number, b: number) {
  return a + b; // inferred return type: number
}

// Object type inference
const user = {
  name: "Alice",
  age: 25,
};
// Inferred type: { name: string; age: number; }

// Best practice: Explicit for parameters, inferred for returns
function multiply(a: number, b: number) {
  return a * b; // Return type inferred as number
}
```

### 6.6 Type Assertion với từ khóa `as`

#### Concept

Type Assertion cho TypeScript biết "Trust me, I know this type".

#### Examples

```typescript
// Type Assertion với 'as'
let someValue: unknown = "this is a string";
let strLength: number = (someValue as string).length;

// DOM manipulation
const input = document.getElementById("username") as HTMLInputElement;
input.value = "Alice";

// API response
interface ApiResponse {
  data: User[];
  total: number;
}

async function fetchUsers() {
  const response = await fetch("/api/users");
  const data = (await response.json()) as ApiResponse;
  return data;
}

// const assertion
const colors = ["red", "green", "blue"] as const;
// type: readonly ["red", "green", "blue"]

// Non-null assertion (!)
function getValue(id?: string) {
  const element = document.getElementById(id!); // Tell TS: id is not null
  return element!.innerHTML; // Tell TS: element is not null
}

// WARNING: Use assertion carefully!
let value: unknown = "hello";
let num: number = value as number; // ❌ Runtime error!
```

---

## 7. Advanced OOP Concepts

### 7.1 OOP (Object-Oriented Programming)

#### Concept

4 nguyên lý OOP:

1. **Encapsulation** (Đóng gói): Ẩn internal state
2. **Inheritance** (Kế thừa): Tái sử dụng code
3. **Polymorphism** (Đa hình): Nhiều hình thức khác nhau
4. **Abstraction** (Trừu tượng hóa): Ẩn complexity

#### Examples

```typescript
// Encapsulation
class User {
  private _password: string;

  constructor(public username: string, password: string) {
    this._password = this.hashPassword(password);
  }

  private hashPassword(password: string): string {
    return `hashed_${password}`;
  }

  verifyPassword(password: string): boolean {
    return this.hashPassword(password) === this._password;
  }
}

// Inheritance
class Animal {
  constructor(protected name: string) {}

  move(distance: number): void {
    console.log(`${this.name} moved ${distance}m`);
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Woof! Woof!");
  }
}

const dog = new Dog("Buddy");
dog.move(10);
dog.bark();

// Polymorphism
interface Shape {
  calculateArea(): number;
}

class Circle implements Shape {
  constructor(private radius: number) {}

  calculateArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}

  calculateArea(): number {
    return this.width * this.height;
  }
}

function printArea(shape: Shape): void {
  console.log(`Area: ${shape.calculateArea()}`);
}

printArea(new Circle(5));
printArea(new Rectangle(4, 6));

// Abstraction
abstract class Vehicle {
  constructor(protected brand: string) {}

  abstract startEngine(): void;

  displayBrand(): void {
    console.log(`Brand: ${this.brand}`);
  }
}

class Car extends Vehicle {
  startEngine(): void {
    console.log("Car engine started: Vroom!");
  }
}

class Motorcycle extends Vehicle {
  startEngine(): void {
    console.log("Motorcycle engine started: Vrrrr!");
  }
}
```

### 7.2 Abstract Class

_(Đã đề cập ở phần trên)_

### 7.3 Decorator

#### Concept

Decorators là functions để modify classes, methods, properties (experimental feature).

#### Examples

```typescript
// Enable decorators in tsconfig.json:
// "experimentalDecorators": true

// Class Decorator
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class BugReport {
  type = "report";
  title: string;

  constructor(title: string) {
    this.title = title;
  }
}

// Method Decorator
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with args:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };

  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(5, 3);
// Log: "Calling add with args: [5, 3]"
// Log: "Result: 8"

// Property Decorator
function readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false,
  });
}

class Person {
  @readonly
  name: string = "Alice";
}

// Parameter Decorator
function required(target: any, propertyKey: string, parameterIndex: number) {
  console.log(`Parameter ${parameterIndex} of ${propertyKey} is required`);
}

class Greeter {
  greet(@required name: string) {
    return `Hello, ${name}`;
  }
}
```

### 7.4 Generic Type

#### Concept

Generics cho phép tạo reusable components với nhiều types khác nhau.

#### Examples

```typescript
// Basic Generic Function
function identity<T>(arg: T): T {
  return arg;
}

const num = identity<number>(42);
const str = identity<string>("hello");
const auto = identity(true); // Type inference

// Generic Array Function
function getFirst<T>(arr: T[]): T | undefined {
  return arr[0];
}

const firstNumber = getFirst([1, 2, 3]); // number | undefined
const firstName = getFirst(["a", "b"]); // string | undefined

// Generic Interface
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

interface User {
  id: number;
  name: string;
}

const userResponse: ApiResponse<User> = {
  data: { id: 1, name: "Alice" },
  status: 200,
  message: "Success",
};

const usersResponse: ApiResponse<User[]> = {
  data: [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
  ],
  status: 200,
  message: "Success",
};

// Generic Class
class Box<T> {
  private value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }

  setValue(value: T): void {
    this.value = value;
  }
}

const numberBox = new Box<number>(123);
const stringBox = new Box<string>("hello");

// Generic Constraints
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): void {
  console.log(arg.length);
}

logLength("hello"); // ✅ string has length
logLength([1, 2, 3]); // ✅ array has length
// logLength(123); // ❌ number doesn't have length

// Multiple Type Parameters
function merge<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = merge({ name: "Alice" }, { age: 25 });
// { name: string; age: number; }

// Generic Constraints với keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { id: 1, name: "Alice", email: "alice@example.com" };
const name = getProperty(user, "name"); // string
// const invalid = getProperty(user, "age"); // ❌ Error
```

---

## Practice Exercises

### Bài 1: Basic Types

```typescript
// Định nghĩa interface cho Student
interface Student {
  // Your code here
}

// Tạo function tính điểm trung bình
function calculateAverage(student: Student): number {
  // Your code here
}
```

### Bài 2: Generics

```typescript
// Tạo generic function để filter array
function filterArray<T>(arr: T[], predicate: (item: T) => boolean): T[] {
  // Your code here
}

// Test
const numbers = [1, 2, 3, 4, 5, 6];
const evenNumbers = filterArray(numbers, (n) => n % 2 === 0);
```

### Bài 3: Class & OOP

```typescript
// Tạo class hierarchy cho e-commerce system
// Abstract class Product
// Class PhysicalProduct extends Product
// Class DigitalProduct extends Product
// Interface Purchasable
```

### Bài 4: Utility Types

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  inStock: boolean;
}

// Tạo type cho product update (tất cả fields optional)
// Tạo type cho product preview (chỉ id, name, price)
// Tạo readonly product type
```

---

## References

1. [TypeScript Official Documentation](https://www.typescriptlang.org/docs/)
2. [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
3. [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
4. [TypeScript Playground](https://www.typescriptlang.org/play)

---

**Previous Module:** [← HTML, CSS](./02-html-css.md)  
**Next Module:** [ReactJS + Restful API →](./04-reactjs-restful-api.md)

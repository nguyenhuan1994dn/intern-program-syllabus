# Module 8: Clean Code and Design Pattern

## Module Objectives

This module helps you write clean code, easy to read, easy to maintain and apply common design patterns in software development.

---

## 1. Naming Convention (Function, Variable, Class...)

### Concept

**Naming Convention** is a rule for naming that helps code be consistent and easy to understand.

**General Rules:**

1. **Descriptive & Meaningful**: Name must describe the purpose clearly
2. **Consistent**: Consistent throughout the codebase
3. **Avoid Abbreviations**: Avoid confusing abbreviations
4. **Use Pronounceable Names**: Easy to read, easy to pronounce

**JavaScript/TypeScript Conventions:**

- **Variables**: camelCase (`userName`, `totalPrice`)
- **Functions**: camelCase (`getUserData`, `calculateTotal`)
- **Classes**: PascalCase (`UserService`, `ProductManager`)
- **Constants**: UPPER_SNAKE_CASE (`API_URL`, `MAX_RETRY`)
- **Private members**: Prefix with `_` or `#` (`_privateMethod`, `#privateField`)
- **Boolean**: Prefix with is/has/should (`isActive`, `hasPermission`, `shouldUpdate`)
- **Interfaces (TS)**: PascalCase, optional I prefix (`User` or `IUser`)
- **Types (TS)**: PascalCase (`UserRole`, `ApiResponse`)
- **Enums**: PascalCase (`Status`, `UserRole`)

### Examples

```typescript
// ========== BAD NAMING ==========
// ❌ Too short, not descriptive
let d = new Date();
let n = "John";
function calc(a, b) { return a + b; }

// ❌ Abbreviations
let usrNm = "John";
let pdtMgr = new ProductManager();
function gtUsrDt() {}

// ❌ Inconsistent
let user_name = "John";  // snake_case
let UserAge = 25;        // PascalCase for variable
let USEREMAIL = "john@example.com";

// ❌ Meaningless names
let data;
let temp;
let foo;
let handleData();

// ========== GOOD NAMING ==========

// ✅ Variables
const userName = "John";
const userAge = 25;
const isActive = true;
const hasPermission = false;
const totalPrice = 100;
const maxRetries = 3;

// ✅ Constants
const API_BASE_URL = "https://api.example.com";
const MAX_FILE_SIZE = 5 * 1024 * 1024; // 5MB
const DEFAULT_TIMEOUT = 30000;

// ✅ Functions
function getUserById(id: number): User { }
function calculateTotalPrice(items: Item[]): number { }
function validateEmail(email: string): boolean { }
function formatCurrency(amount: number): string { }

// ✅ Boolean functions/variables
function isValidEmail(email: string): boolean { }
function hasAdminRole(user: User): boolean { }
function shouldShowModal(): boolean { }

const isLoading = false;
const hasError = false;
const canEdit = true;

// ✅ Classes
class UserService { }
class ProductManager { }
class EmailValidator { }
class HttpClient { }

// ✅ Interfaces & Types
interface User {
  id: number;
  name: string;
  email: string;
}

type UserRole = "admin" | "user" | "guest";
type ApiResponse<T> = {
  data: T;
  status: number;
};

// ✅ Enums
enum Status {
  Pending = "PENDING",
  Approved = "APPROVED",
  Rejected = "REJECTED"
}

enum HttpMethod {
  GET = "GET",
  POST = "POST",
  PUT = "PUT",
  DELETE = "DELETE"
}

// ✅ Private members
class BankAccount {
  private _balance: number;
  #privateKey: string;

  private _calculateInterest(): number { }
  #encryptData(data: string): string { }
}

// ========== CONTEXT-SPECIFIC NAMING ==========

// Event Handlers
function handleClick() { }
function handleSubmit() { }
function handleInputChange() { }
function onUserLogin() { }
function onDataLoad() { }

// React Components
function UserProfile() { }
function NavigationBar() { }
function ProductCard() { }

// React Hooks
function useUser() { }
function useFetchData() { }
function useLocalStorage() { }

// API Functions
async function fetchUsers(): Promise<User[]> { }
async function createUser(data: CreateUserDto): Promise<User> { }
async function updateUser(id: number, data: UpdateUserDto): Promise<User> { }
async function deleteUser(id: number): Promise<void> { }

// Utility Functions
function formatDate(date: Date): string { }
function parseJSON(jsonString: string): any { }
function deepClone<T>(obj: T): T { }

// ========== AVOID MAGIC NUMBERS ==========

// ❌ Bad
if (user.role === 1) { }
setTimeout(() => {}, 5000);

// ✅ Good
const ROLE_ADMIN = 1;
const TOAST_DURATION = 5000;

if (user.role === ROLE_ADMIN) { }
setTimeout(() => {}, TOAST_DURATION);

// Even better: Use enum
enum Role {
  Admin = 1,
  User = 2,
  Guest = 3
}

if (user.role === Role.Admin) { }
```

---

## 2. Structure

### Concept

**Code Structure** helps organize code logically and maintainably.

**Project Structure Best Practices:**

- **Feature-based**: Group by features instead of types
- **Separation of Concerns**: Separate logic, UI, data
- **Single Responsibility**: Each file/module has one responsibility
- **DRY** (Don't Repeat Yourself): Avoid duplicate code

### Examples

```
// ========== BAD STRUCTURE (Type-based) ==========
src/
├── components/
│   ├── UserList.tsx
│   ├── UserDetail.tsx
│   ├── ProductList.tsx
│   └── ProductDetail.tsx
├── services/
│   ├── UserService.ts
│   └── ProductService.ts
├── types/
│   ├── User.ts
│   └── Product.ts
└── utils/
    └── helpers.ts

// Problem: Related code scattered across folders

// ========== GOOD STRUCTURE (Feature-based) ==========
src/
├── features/
│   ├── user/
│   │   ├── components/
│   │   │   ├── UserList.tsx
│   │   │   ├── UserDetail.tsx
│   │   │   └── UserForm.tsx
│   │   ├── hooks/
│   │   │   ├── useUser.ts
│   │   │   └── useUsers.ts
│   │   ├── services/
│   │   │   └── userService.ts
│   │   ├── types/
│   │   │   └── user.types.ts
│   │   └── utils/
│   │       └── userHelpers.ts
│   │
│   └── product/
│       ├── components/
│       ├── hooks/
│       ├── services/
│       ├── types/
│       └── utils/
│
├── shared/
│   ├── components/         # Shared UI components
│   │   ├── Button.tsx
│   │   ├── Modal.tsx
│   │   └── Input.tsx
│   ├── hooks/              # Shared hooks
│   │   ├── useLocalStorage.ts
│   │   └── useDebounce.ts
│   ├── utils/              # Shared utilities
│   │   ├── formatDate.ts
│   │   └── validators.ts
│   └── types/              # Shared types
│       └── common.types.ts
│
├── layouts/
│   ├── MainLayout.tsx
│   └── AuthLayout.tsx
│
├── pages/
│   ├── HomePage.tsx
│   ├── UserPage.tsx
│   └── ProductPage.tsx
│
├── config/
│   ├── api.config.ts
│   └── app.config.ts
│
└── App.tsx
```

```typescript
// ========== FILE STRUCTURE EXAMPLE ==========

// ===== features/user/types/user.types.ts =====
export interface User {
  id: number;
  name: string;
  email: string;
  role: UserRole;
}

export type UserRole = "admin" | "user" | "guest";

export interface CreateUserDto {
  name: string;
  email: string;
  password: string;
}

// ===== features/user/services/userService.ts =====
import { User, CreateUserDto } from "../types/user.types";

class UserService {
  private baseURL = "/api/users";

  async getAll(): Promise<User[]> {
    const response = await fetch(this.baseURL);
    return response.json();
  }

  async getById(id: number): Promise<User> {
    const response = await fetch(`${this.baseURL}/${id}`);
    return response.json();
  }

  async create(data: CreateUserDto): Promise<User> {
    const response = await fetch(this.baseURL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });
    return response.json();
  }
}

export const userService = new UserService();

// ===== features/user/hooks/useUsers.ts =====
import { useState, useEffect } from "react";
import { userService } from "../services/userService";
import { User } from "../types/user.types";

export function useUsers() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    loadUsers();
  }, []);

  async function loadUsers() {
    try {
      setLoading(true);
      const data = await userService.getAll();
      setUsers(data);
    } catch (err) {
      setError("Failed to load users");
    } finally {
      setLoading(false);
    }
  }

  return { users, loading, error, refetch: loadUsers };
}

// ===== features/user/components/UserList.tsx =====
import { useUsers } from "../hooks/useUsers";

export function UserList() {
  const { users, loading, error } = useUsers();

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// ===== Separation of Concerns Example =====

// ❌ BAD: Everything in one file
function UserManagement() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then((data) => setUsers(data));
  }, []);

  const handleCreate = (data) => {
    fetch("/api/users", {
      method: "POST",
      body: JSON.stringify(data),
    });
  };

  return <div>{/* Lots of JSX here */}</div>;
}

// ✅ GOOD: Separated into layers

// Service Layer (API calls)
const userService = {
  getAll: () => fetch("/api/users").then((r) => r.json()),
  create: (data) =>
    fetch("/api/users", {
      method: "POST",
      body: JSON.stringify(data),
    }),
};

// Hook Layer (Business logic)
function useUsers() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    userService.getAll().then(setUsers);
  }, []);

  const createUser = (data) => {
    return userService.create(data).then((newUser) => {
      setUsers([...users, newUser]);
    });
  };

  return { users, createUser };
}

// Component Layer (UI)
function UserManagement() {
  const { users, createUser } = useUsers();

  return (
    <div>
      <UserList users={users} />
      <UserForm onSubmit={createUser} />
    </div>
  );
}
```

---

## 3. Eslint, Prettier plugin

### Concept

**ESLint**: Linter to detect errors and enforce code style.  
**Prettier**: Code formatter to automatically format code.

### Setup

```bash
# Install ESLint
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

# Install Prettier
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier

# Install for React
npm install --save-dev eslint-plugin-react eslint-plugin-react-hooks
```

### Configuration

```json
// ===== .eslintrc.json =====
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint", "react", "react-hooks", "prettier"],
  "rules": {
    "prettier/prettier": "error",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-explicit-any": "warn",
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off",
    "no-console": "warn",
    "no-unused-vars": "off",
    "@typescript-eslint/no-unused-vars": ["error", {
      "argsIgnorePattern": "^_"
    }]
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}

// ===== .prettierrc =====
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "endOfLine": "lf"
}

// ===== .prettierignore =====
node_modules
build
dist
coverage
.next
*.min.js

// ===== package.json scripts =====
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,json,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{ts,tsx,json,css,md}\""
  }
}
```

### Examples

```typescript
// ❌ Before Prettier & ESLint
const user={name:"John",age:25,email:"john@example.com"}
function getUser(id:number):Promise<User>
{
const response=await fetch(`/api/users/${id}`)
return response.json()
}

// ✅ After Prettier & ESLint
const user = {
  name: 'John',
  age: 25,
  email: 'john@example.com',
};

async function getUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// ===== VS Code Integration =====
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ]
}
```

---

## 4. Overview Patterns

### Concept

**Design Patterns** are reusable solutions for common problems in software design.

**3 Categories:**

1. **Creational Patterns**: Object creation

   - Singleton, Factory, Builder, Prototype

2. **Structural Patterns**: Object composition

   - Adapter, Decorator, Facade, Composite

3. **Behavioral Patterns**: Object interaction
   - Observer, Strategy, Command, State

---

## 5. Composite, Prototype, Flux Pattern

### 5.1 Composite Pattern

#### Concept

**Composite Pattern** allows composing objects into tree structures to represent part-whole hierarchies.

**Use cases:**

- File system (folders & files)
- UI components (containers & elements)
- Organization structure

#### Examples

```typescript
// ========== COMPOSITE PATTERN ==========

// Component interface
interface FileSystemItem {
  getName(): string;
  getSize(): number;
  print(indent?: string): void;
}

// Leaf (File)
class File implements FileSystemItem {
  constructor(private name: string, private size: number) {}

  getName(): string {
    return this.name;
  }

  getSize(): number {
    return this.size;
  }

  print(indent = ""): void {
    console.log(`${indent}📄 ${this.name} (${this.size}KB)`);
  }
}

// Composite (Folder)
class Folder implements FileSystemItem {
  private items: FileSystemItem[] = [];

  constructor(private name: string) {}

  add(item: FileSystemItem): void {
    this.items.push(item);
  }

  remove(item: FileSystemItem): void {
    const index = this.items.indexOf(item);
    if (index > -1) {
      this.items.splice(index, 1);
    }
  }

  getName(): string {
    return this.name;
  }

  getSize(): number {
    return this.items.reduce((total, item) => total + item.getSize(), 0);
  }

  print(indent = ""): void {
    console.log(`${indent}📁 ${this.name} (${this.getSize()}KB)`);
    this.items.forEach((item) => item.print(indent + "  "));
  }
}

// Usage
const root = new Folder("root");
const documents = new Folder("documents");
const photos = new Folder("photos");

documents.add(new File("resume.pdf", 200));
documents.add(new File("cover-letter.doc", 50));

photos.add(new File("photo1.jpg", 1500));
photos.add(new File("photo2.jpg", 1200));

root.add(documents);
root.add(photos);
root.add(new File("readme.txt", 10));

root.print();
// 📁 root (2960KB)
//   📁 documents (250KB)
//     📄 resume.pdf (200KB)
//     📄 cover-letter.doc (50KB)
//   📁 photos (2700KB)
//     📄 photo1.jpg (1500KB)
//     📄 photo2.jpg (1200KB)
//   📄 readme.txt (10KB)

console.log(`Total size: ${root.getSize()}KB`);

// ========== REACT EXAMPLE ==========
interface ComponentProps {
  children?: React.ReactNode;
}

// Composite component
function Box({ children }: ComponentProps) {
  return <div className="box">{children}</div>;
}

// Leaf components
function Text({ children }: ComponentProps) {
  return <p>{children}</p>;
}

function Image({ src }: { src: string }) {
  return <img src={src} alt="" />;
}

// Usage - Build tree structure
function App() {
  return (
    <Box>
      <Text>Header</Text>
      <Box>
        <Image src="photo.jpg" />
        <Text>Caption</Text>
      </Box>
      <Box>
        <Text>Footer</Text>
      </Box>
    </Box>
  );
}
```

### 5.2 Prototype Pattern

#### Concept

**Prototype Pattern** creates new objects by cloning from a prototype object.

**Use cases:**

- Create multiple similar objects
- Avoid expensive initialization
- Configuration objects

#### Examples

```typescript
// ========== PROTOTYPE PATTERN ==========

// Prototype interface
interface Cloneable<T> {
  clone(): T;
}

// Prototype class
class UserPrototype implements Cloneable<UserPrototype> {
  constructor(
    public name: string,
    public email: string,
    public role: string,
    public permissions: string[]
  ) {}

  clone(): UserPrototype {
    // Deep clone
    return new UserPrototype(this.name, this.email, this.role, [
      ...this.permissions,
    ]);
  }
}

// Usage
const adminPrototype = new UserPrototype(
  "Admin",
  "admin@example.com",
  "admin",
  ["read", "write", "delete"]
);

// Create new admins by cloning
const admin1 = adminPrototype.clone();
admin1.name = "Alice";
admin1.email = "alice@example.com";

const admin2 = adminPrototype.clone();
admin2.name = "Bob";
admin2.email = "bob@example.com";

console.log(admin1);
console.log(admin2);
console.log(adminPrototype); // Original unchanged

// ========== JAVASCRIPT PROTOTYPE ==========
const carPrototype = {
  drive() {
    console.log(`${this.model} is driving`);
  },
  stop() {
    console.log(`${this.model} stopped`);
  },
};

// Create objects from prototype
const car1 = Object.create(carPrototype);
car1.model = "Tesla Model 3";

const car2 = Object.create(carPrototype);
car2.model = "BMW i4";

car1.drive(); // "Tesla Model 3 is driving"
car2.drive(); // "BMW i4 is driving"

// ========== COMPLEX PROTOTYPE ==========
class Shape {
  constructor(public x: number, public y: number, public color: string) {}

  clone(): this {
    const clone = Object.create(Object.getPrototypeOf(this));
    Object.assign(clone, this);
    return clone;
  }
}

class Circle extends Shape {
  constructor(x: number, y: number, color: string, public radius: number) {
    super(x, y, color);
  }

  clone(): this {
    const clone = super.clone();
    clone.radius = this.radius;
    return clone;
  }
}

const circlePrototype = new Circle(0, 0, "red", 10);
const circle1 = circlePrototype.clone();
circle1.x = 100;
circle1.y = 100;

const circle2 = circlePrototype.clone();
circle2.x = 200;
circle2.y = 200;
circle2.color = "blue";
```

### 5.3 Flux Pattern

#### Concept

**Flux** is an application architecture pattern for React, with unidirectional data flow.

**Components:**

1. **Actions**: Events/payloads
2. **Dispatcher**: Central hub
3. **Stores**: Application state
4. **Views**: React components

**Flow:**

```
Action → Dispatcher → Store → View → Action
```

#### Examples

```typescript
// ========== FLUX PATTERN (Simplified) ==========

// 1. Action Types
enum ActionType {
  ADD_TODO = "ADD_TODO",
  TOGGLE_TODO = "TOGGLE_TODO",
  DELETE_TODO = "DELETE_TODO",
}

// 2. Actions
interface Action {
  type: ActionType;
  payload?: any;
}

const TodoActions = {
  addTodo: (text: string): Action => ({
    type: ActionType.ADD_TODO,
    payload: { text },
  }),

  toggleTodo: (id: number): Action => ({
    type: ActionType.TOGGLE_TODO,
    payload: { id },
  }),

  deleteTodo: (id: number): Action => ({
    type: ActionType.DELETE_TODO,
    payload: { id },
  }),
};

// 3. Dispatcher
type Listener = (action: Action) => void;

class Dispatcher {
  private listeners: Listener[] = [];

  register(listener: Listener): void {
    this.listeners.push(listener);
  }

  dispatch(action: Action): void {
    this.listeners.forEach((listener) => listener(action));
  }
}

const dispatcher = new Dispatcher();

// 4. Store
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

class TodoStore {
  private todos: Todo[] = [];
  private listeners: Array<() => void> = [];

  constructor() {
    dispatcher.register((action: Action) => {
      switch (action.type) {
        case ActionType.ADD_TODO:
          this.addTodo(action.payload.text);
          break;
        case ActionType.TOGGLE_TODO:
          this.toggleTodo(action.payload.id);
          break;
        case ActionType.DELETE_TODO:
          this.deleteTodo(action.payload.id);
          break;
      }
    });
  }

  private addTodo(text: string): void {
    this.todos.push({
      id: Date.now(),
      text,
      completed: false,
    });
    this.emitChange();
  }

  private toggleTodo(id: number): void {
    const todo = this.todos.find((t) => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      this.emitChange();
    }
  }

  private deleteTodo(id: number): void {
    this.todos = this.todos.filter((t) => t.id !== id);
    this.emitChange();
  }

  getTodos(): Todo[] {
    return this.todos;
  }

  subscribe(listener: () => void): () => void {
    this.listeners.push(listener);
    return () => {
      this.listeners = this.listeners.filter((l) => l !== listener);
    };
  }

  private emitChange(): void {
    this.listeners.forEach((listener) => listener());
  }
}

const todoStore = new TodoStore();

// 5. View (React Component)
function TodoApp() {
  const [todos, setTodos] = useState<Todo[]>([]);

  useEffect(() => {
    const updateTodos = () => {
      setTodos(todoStore.getTodos());
    };

    const unsubscribe = todoStore.subscribe(updateTodos);
    updateTodos(); // Initial load

    return unsubscribe;
  }, []);

  const handleAddTodo = (text: string) => {
    dispatcher.dispatch(TodoActions.addTodo(text));
  };

  const handleToggleTodo = (id: number) => {
    dispatcher.dispatch(TodoActions.toggleTodo(id));
  };

  const handleDeleteTodo = (id: number) => {
    dispatcher.dispatch(TodoActions.deleteTodo(id));
  };

  return (
    <div>
      <TodoInput onAdd={handleAddTodo} />
      <TodoList
        todos={todos}
        onToggle={handleToggleTodo}
        onDelete={handleDeleteTodo}
      />
    </div>
  );
}

// ========== MODERN FLUX: Redux (Simplified) ==========
// Redux is a Flux implementation

interface State {
  todos: Todo[];
}

type TodoAction =
  | { type: "ADD_TODO"; payload: string }
  | { type: "TOGGLE_TODO"; payload: number }
  | { type: "DELETE_TODO"; payload: number };

// Reducer (like Store)
function todoReducer(state: State = { todos: [] }, action: TodoAction): State {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [
          ...state.todos,
          { id: Date.now(), text: action.payload, completed: false },
        ],
      };

    case "TOGGLE_TODO":
      return {
        ...state,
        todos: state.todos.map((todo) =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        ),
      };

    case "DELETE_TODO":
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload),
      };

    default:
      return state;
  }
}

// Usage with Redux
/*
import { createStore } from 'redux';

const store = createStore(todoReducer);

// Dispatch actions
store.dispatch({ type: 'ADD_TODO', payload: 'Learn Redux' });

// Get state
console.log(store.getState());

// Subscribe to changes
store.subscribe(() => {
  console.log('State updated:', store.getState());
});
*/
```

---

## Tổng hợp Best Practices

```typescript
// ========== CLEAN CODE PRINCIPLES ==========

// 1. ✅ Single Responsibility
// Each function/class does ONE thing well

// ❌ Bad
function handleUser(user: User) {
  validateUser(user);
  saveToDatabase(user);
  sendWelcomeEmail(user);
  logActivity(user);
}

// ✅ Good
function validateUser(user: User): boolean {}
function saveUser(user: User): Promise<void> {}
function sendWelcomeEmail(user: User): Promise<void> {}
function logUserActivity(user: User): void {}

// 2. ✅ DRY (Don't Repeat Yourself)

// ❌ Bad
function calculateTotalWithTax(price: number): number {
  return price + price * 0.1;
}

function calculateDiscountedPriceWithTax(
  price: number,
  discount: number
): number {
  const discounted = price - price * discount;
  return discounted + discounted * 0.1;
}

// ✅ Good
const TAX_RATE = 0.1;

function applyTax(price: number): number {
  return price + price * TAX_RATE;
}

function calculateTotal(price: number, discount = 0): number {
  const discounted = price - price * discount;
  return applyTax(discounted);
}

// 3. ✅ Meaningful Names

// ❌ Bad
const d = new Date();
const arr = users.filter((u) => u.a > 18);

// ✅ Good
const currentDate = new Date();
const adultUsers = users.filter((user) => user.age > 18);

// 4. ✅ Small Functions

// ❌ Bad
function processOrder(order: Order) {
  // 100 lines of code...
}

// ✅ Good
function processOrder(order: Order) {
  validateOrder(order);
  calculateTotal(order);
  applyDiscount(order);
  processPayment(order);
  sendConfirmation(order);
}

// 5. ✅ Error Handling

// ❌ Bad
function getUser(id: number) {
  const user = database.find(id);
  return user.name; // Crash if user is null!
}

// ✅ Good
function getUser(id: number): User | null {
  const user = database.find(id);
  if (!user) {
    return null;
  }
  return user;
}

// Or with error
async function getUser(id: number): Promise<User> {
  const user = await database.find(id);
  if (!user) {
    throw new Error(`User with ID ${id} not found`);
  }
  return user;
}

// 6. ✅ Comments (when necessary)

// ❌ Bad
// Increment i
i++;

// ✅ Good
// Apply 10% discount for orders over $100
const discount = order.total > 100 ? 0.1 : 0;

// 7. ✅ Consistent Formatting
// Use Prettier + ESLint

// 8. ✅ SOLID Principles
// S: Single Responsibility
// O: Open/Closed
// L: Liskov Substitution
// I: Interface Segregation
// D: Dependency Inversion
```

---

## Practice Exercises

### Exercise 1: Refactor Code

Refactor the following code according to clean code principles:

```typescript
function p(u) {
  if (u.a > 18 && u.e.includes("@")) {
    let d = new Date();
    // Save user logic
    return true;
  }
  return false;
}
```

### Exercise 2: Implement Composite Pattern

Create a component tree for navigation menu with nested items.

### Exercise 3: Setup ESLint & Prettier

Setup ESLint and Prettier for React TypeScript project.

### Exercise 4: Flux Pattern

Implement simple counter app using Flux pattern.

---

## References

1. [Clean Code by Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
2. [Design Patterns: Elements of Reusable Object-Oriented Software](https://refactoring.guru/design-patterns)
3. [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
4. [ESLint Documentation](https://eslint.org/docs/latest/)
5. [Prettier Documentation](https://prettier.io/docs/en/index.html)

---

**Previous Module:** [← GraphQL](./05-graphql.md)  
**[Back to Main README](../README.md)**

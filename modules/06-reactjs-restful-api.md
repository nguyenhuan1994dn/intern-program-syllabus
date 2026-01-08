# Module 6: ReactJS + Restful API

## Mục tiêu module

Module này giúp bạn nắm vững ReactJS - thư viện UI phổ biến nhất hiện nay, cùng với cách tích hợp RESTful API.

---

## 1. Restful API

###1.1 Khái niệm

**REST** (Representational State Transfer) là một kiến trúc cho web services sử dụng HTTP methods.

**6 nguyên tắc REST:**

1. **Client-Server**: Tách biệt client và server
2. **Stateless**: Mỗi request độc lập, không lưu state
3. **Cacheable**: Response có thể cache được
4. **Uniform Interface**: Interface thống nhất
5. **Layered System**: Hệ thống nhiều layer
6. **Code on Demand** (optional): Server có thể gửi code

**HTTP Methods:**

- `GET`: Lấy dữ liệu
- `POST`: Tạo mới
- `PUT`: Cập nhật toàn bộ
- `PATCH`: Cập nhật một phần
- `DELETE`: Xóa

**Status Codes:**

- `200 OK`: Thành công
- `201 Created`: Tạo thành công
- `400 Bad Request`: Request sai
- `401 Unauthorized`: Chưa xác thực
- `403 Forbidden`: Không có quyền
- `404 Not Found`: Không tìm thấy
- `500 Internal Server Error`: Lỗi server

### 1.2 Ví dụ

```typescript
// API Endpoints Examples
const API_BASE_URL = "https://api.example.com";

// GET - Lấy danh sách users
GET /api/users
Response: 200 OK
{
  "data": [
    { "id": 1, "name": "Alice", "email": "alice@example.com" },
    { "id": 2, "name": "Bob", "email": "bob@example.com" }
  ],
  "total": 2
}

// GET - Lấy user theo ID
GET /api/users/1
Response: 200 OK
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com",
  "role": "admin"
}

// POST - Tạo user mới
POST /api/users
Body: {
  "name": "Charlie",
  "email": "charlie@example.com",
  "password": "secret123"
}
Response: 201 Created
{
  "id": 3,
  "name": "Charlie",
  "email": "charlie@example.com"
}

// PUT - Cập nhật toàn bộ user
PUT /api/users/1
Body: {
  "name": "Alice Updated",
  "email": "alice.new@example.com",
  "role": "user"
}
Response: 200 OK

// PATCH - Cập nhật một phần
PATCH /api/users/1
Body: {
  "role": "admin"
}
Response: 200 OK

// DELETE - Xóa user
DELETE /api/users/1
Response: 204 No Content

// Query Parameters
GET /api/users?page=1&limit=10&sort=name&order=asc

// Nested Resources
GET /api/users/1/posts
GET /api/posts/1/comments

// Error Response
Response: 400 Bad Request
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": {
      "field": "email",
      "value": "invalid-email"
    }
  }
}
```

### 1.3 Fetch API trong JavaScript/TypeScript

```typescript
// ========== INTERFACES ==========
interface User {
  id: number;
  name: string;
  email: string;
  role?: string;
}

interface ApiResponse<T> {
  data: T;
  message?: string;
}

interface ApiError {
  error: {
    code: string;
    message: string;
  };
}

// ========== GET REQUEST ==========
async function getUsers(): Promise<User[]> {
  try {
    const response = await fetch(`${API_BASE_URL}/users`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data: ApiResponse<User[]> = await response.json();
    return data.data;
  } catch (error) {
    console.error("Failed to fetch users:", error);
    throw error;
  }
}

// ========== GET BY ID ==========
async function getUserById(id: number): Promise<User> {
  const response = await fetch(`${API_BASE_URL}/users/${id}`);

  if (!response.ok) {
    if (response.status === 404) {
      throw new Error("User not found");
    }
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  return response.json();
}

// ========== POST REQUEST ==========
async function createUser(userData: Omit<User, "id">): Promise<User> {
  const response = await fetch(`${API_BASE_URL}/users`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(userData),
  });

  if (!response.ok) {
    const error: ApiError = await response.json();
    throw new Error(error.error.message);
  }

  return response.json();
}

// ========== PUT REQUEST ==========
async function updateUser(id: number, userData: Partial<User>): Promise<User> {
  const response = await fetch(`${API_BASE_URL}/users/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(userData),
  });

  if (!response.ok) {
    throw new Error(`Failed to update user: ${response.status}`);
  }

  return response.json();
}

// ========== DELETE REQUEST ==========
async function deleteUser(id: number): Promise<void> {
  const response = await fetch(`${API_BASE_URL}/users/${id}`, {
    method: "DELETE",
  });

  if (!response.ok) {
    throw new Error(`Failed to delete user: ${response.status}`);
  }
}

// ========== WITH AUTHENTICATION ==========
async function fetchWithAuth(
  url: string,
  options: RequestInit = {}
): Promise<Response> {
  const token = localStorage.getItem("authToken");

  const headers = {
    "Content-Type": "application/json",
    ...(token && { Authorization: `Bearer ${token}` }),
    ...options.headers,
  };

  return fetch(url, { ...options, headers });
}

// Usage
async function getProtectedData() {
  const response = await fetchWithAuth(`${API_BASE_URL}/protected`);
  return response.json();
}

// ========== ERROR HANDLING ==========
async function fetchUsersWithErrorHandling() {
  try {
    const response = await fetch(`${API_BASE_URL}/users`);

    // Check for HTTP errors
    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.error?.message || "Request failed");
    }

    const data = await response.json();
    return data;
  } catch (error) {
    if (error instanceof TypeError) {
      // Network error
      console.error("Network error:", error.message);
    } else {
      // Other errors
      console.error("Error:", error);
    }
    throw error;
  }
}
```

---

## 2. Header, Content Type, Allow Origin

### Khái niệm

**Headers** chứa metadata của request/response.

**Common Headers:**

- `Content-Type`: Định dạng dữ liệu
- `Authorization`: Xác thực
- `Accept`: Định dạng response mong muốn
- `Access-Control-Allow-Origin`: CORS policy

**Content Types:**

- `application/json`: JSON data
- `application/x-www-form-urlencoded`: Form data
- `multipart/form-data`: File upload
- `text/plain`: Plain text
- `text/html`: HTML

**CORS** (Cross-Origin Resource Sharing): Cho phép requests từ domain khác.

### Ví dụ

```typescript
// ========== CONTENT-TYPE EXAMPLES ==========

// JSON Request
async function sendJSON() {
  const response = await fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      name: "Alice",
      email: "alice@example.com",
    }),
  });
}

// Form Data Request
async function sendFormData() {
  const formData = new FormData();
  formData.append("name", "Alice");
  formData.append("email", "alice@example.com");

  const response = await fetch("/api/users", {
    method: "POST",
    // Don't set Content-Type for FormData, browser sets it automatically
    body: formData,
  });
}

// File Upload
async function uploadFile(file: File) {
  const formData = new FormData();
  formData.append("file", file);
  formData.append("description", "My file");

  const response = await fetch("/api/upload", {
    method: "POST",
    body: formData,
  });

  return response.json();
}

// ========== CUSTOM HEADERS ==========
async function fetchWithCustomHeaders() {
  const response = await fetch("/api/data", {
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "X-Custom-Header": "custom-value",
      "Accept-Language": "en-US",
    },
  });
}

// ========== CORS EXAMPLE ==========
// Server-side (Express.js)
/*
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*'); // Allow all origins
  // Or specific origin:
  // res.header('Access-Control-Allow-Origin', 'https://example.com');
  
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, PATCH');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  res.header('Access-Control-Allow-Credentials', 'true');
  next();
});
*/

// Client-side (with credentials)
async function fetchWithCredentials() {
  const response = await fetch("https://api.example.com/data", {
    method: "GET",
    credentials: "include", // Send cookies
    headers: {
      "Content-Type": "application/json",
    },
  });
}

// ========== READING RESPONSE HEADERS ==========
async function readResponseHeaders() {
  const response = await fetch("/api/data");

  // Get specific header
  const contentType = response.headers.get("Content-Type");
  const rateLimit = response.headers.get("X-RateLimit-Remaining");

  // Iterate all headers
  response.headers.forEach((value, key) => {
    console.log(`${key}: ${value}`);
  });
}
```

---

## 3. Error Status (401, 403)

### Khái niệm

**Authentication vs Authorization:**

- **401 Unauthorized**: Chưa đăng nhập (authentication failed)
- **403 Forbidden**: Đã đăng nhập nhưng không có quyền (authorization failed)

**Other Common Errors:**

- **400 Bad Request**: Request sai format
- **404 Not Found**: Resource không tồn tại
- **409 Conflict**: Conflict (e.g., duplicate email)
- **422 Unprocessable Entity**: Validation error
- **429 Too Many Requests**: Rate limit exceeded
- **500 Internal Server Error**: Lỗi server
- **503 Service Unavailable**: Service tạm thời không khả dụng

### Ví dụ

```typescript
// ========== ERROR HANDLING ==========
class ApiError extends Error {
  constructor(public status: number, public code: string, message: string) {
    super(message);
    this.name = "ApiError";
  }
}

async function fetchApi<T>(url: string, options: RequestInit = {}): Promise<T> {
  try {
    const response = await fetch(url, options);

    // Handle different status codes
    if (response.status === 401) {
      // Redirect to login
      window.location.href = "/login";
      throw new ApiError(401, "UNAUTHORIZED", "Please log in");
    }

    if (response.status === 403) {
      throw new ApiError(403, "FORBIDDEN", "You do not have permission");
    }

    if (response.status === 404) {
      throw new ApiError(404, "NOT_FOUND", "Resource not found");
    }

    if (response.status === 429) {
      throw new ApiError(429, "RATE_LIMIT", "Too many requests");
    }

    if (response.status >= 500) {
      throw new ApiError(response.status, "SERVER_ERROR", "Server error");
    }

    if (!response.ok) {
      const error = await response.json();
      throw new ApiError(response.status, error.code, error.message);
    }

    return response.json();
  } catch (error) {
    if (error instanceof ApiError) {
      throw error;
    }

    // Network error
    throw new ApiError(0, "NETWORK_ERROR", "Network connection failed");
  }
}

// Usage
async function loadUserData() {
  try {
    const user = await fetchApi<User>("/api/user/profile");
    console.log(user);
  } catch (error) {
    if (error instanceof ApiError) {
      switch (error.status) {
        case 401:
          alert("Please log in");
          break;
        case 403:
          alert("Access denied");
          break;
        case 404:
          alert("User not found");
          break;
        default:
          alert("An error occurred");
      }
    }
  }
}

// ========== RETRY LOGIC ==========
async function fetchWithRetry<T>(
  url: string,
  options: RequestInit = {},
  maxRetries = 3
): Promise<T> {
  let lastError: Error;

  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);

      // Don't retry on client errors (4xx)
      if (response.status >= 400 && response.status < 500) {
        throw new Error(`Client error: ${response.status}`);
      }

      if (!response.ok) {
        throw new Error(`HTTP error: ${response.status}`);
      }

      return response.json();
    } catch (error) {
      lastError = error as Error;

      // Wait before retry (exponential backoff)
      if (i < maxRetries - 1) {
        await new Promise((resolve) =>
          setTimeout(resolve, Math.pow(2, i) * 1000)
        );
      }
    }
  }

  throw lastError!;
}
```

---

## 4. Response, Request

### Khái niệm

**Request** gồm:

- Method (GET, POST, etc.)
- URL
- Headers
- Body (optional)

**Response** gồm:

- Status Code
- Headers
- Body

### Ví dụ

```typescript
// ========== REQUEST INTERCEPTOR ==========
class ApiClient {
  private baseURL: string;
  private defaultHeaders: HeadersInit;

  constructor(baseURL: string) {
    this.baseURL = baseURL;
    this.defaultHeaders = {
      "Content-Type": "application/json",
    };
  }

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    const url = `${this.baseURL}${endpoint}`;

    // Add default headers
    const headers = {
      ...this.defaultHeaders,
      ...options.headers,
    };

    // Add auth token if available
    const token = localStorage.getItem("token");
    if (token) {
      headers["Authorization"] = `Bearer ${token}`;
    }

    // Log request
    console.log(`[Request] ${options.method || "GET"} ${url}`);

    const response = await fetch(url, {
      ...options,
      headers,
    });

    // Log response
    console.log(`[Response] ${response.status} ${url}`);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return response.json();
  }

  async get<T>(endpoint: string): Promise<T> {
    return this.request<T>(endpoint, { method: "GET" });
  }

  async post<T>(endpoint: string, data: any): Promise<T> {
    return this.request<T>(endpoint, {
      method: "POST",
      body: JSON.stringify(data),
    });
  }

  async put<T>(endpoint: string, data: any): Promise<T> {
    return this.request<T>(endpoint, {
      method: "PUT",
      body: JSON.stringify(data),
    });
  }

  async delete<T>(endpoint: string): Promise<T> {
    return this.request<T>(endpoint, { method: "DELETE" });
  }
}

// Usage
const api = new ApiClient("https://api.example.com");

// GET request
const users = await api.get<User[]>("/users");

// POST request
const newUser = await api.post<User>("/users", {
  name: "Alice",
  email: "alice@example.com",
});

// PUT request
const updatedUser = await api.put<User>("/users/1", {
  name: "Alice Updated",
});

// DELETE request
await api.delete("/users/1");
```

---

## 5. Advance (FormData, XML Request)

### Khái niệm

**FormData**: API để gửi form data và files.
**XMLHttpRequest**: API cũ để thực hiện HTTP requests (trước Fetch API).

### Ví dụ

```typescript
// ========== FORMDATA ==========

// Simple Form Data
const formData = new FormData();
formData.append("username", "alice");
formData.append("email", "alice@example.com");

await fetch("/api/users", {
  method: "POST",
  body: formData,
});

// File Upload with FormData
async function uploadAvatar(file: File, userId: number) {
  const formData = new FormData();
  formData.append("avatar", file);
  formData.append("userId", userId.toString());

  const response = await fetch("/api/upload/avatar", {
    method: "POST",
    body: formData,
  });

  return response.json();
}

// Multiple Files
async function uploadMultipleFiles(files: FileList) {
  const formData = new FormData();

  Array.from(files).forEach((file, index) => {
    formData.append(`file${index}`, file);
  });

  formData.append("description", "Multiple files upload");

  const response = await fetch("/api/upload/multiple", {
    method: "POST",
    body: formData,
  });

  return response.json();
}

// Convert Form Element to FormData
const form = document.querySelector("form")!;
const formData2 = new FormData(form);

// Add additional fields
formData2.append("timestamp", Date.now().toString());

// ========== XMLHTTPREQUEST (Legacy) ==========
function fetchWithXHR(url: string): Promise<any> {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open("GET", url);

    // Set headers
    xhr.setRequestHeader("Content-Type", "application/json");

    // Handle response
    xhr.onload = () => {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(JSON.parse(xhr.responseText));
      } else {
        reject(new Error(`HTTP ${xhr.status}: ${xhr.statusText}`));
      }
    };

    // Handle error
    xhr.onerror = () => {
      reject(new Error("Network error"));
    };

    // Track progress
    xhr.onprogress = (event) => {
      if (event.lengthComputable) {
        const percentComplete = (event.loaded / event.total) * 100;
        console.log(`Progress: ${percentComplete}%`);
      }
    };

    xhr.send();
  });
}

// File Upload with Progress
function uploadFileWithProgress(
  file: File,
  onProgress: (percent: number) => void
): Promise<any> {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    const formData = new FormData();
    formData.append("file", file);

    // Upload progress
    xhr.upload.onprogress = (event) => {
      if (event.lengthComputable) {
        const percent = (event.loaded / event.total) * 100;
        onProgress(percent);
      }
    };

    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.responseText));
      } else {
        reject(new Error(`Upload failed: ${xhr.status}`));
      }
    };

    xhr.onerror = () => reject(new Error("Upload failed"));

    xhr.open("POST", "/api/upload");
    xhr.send(formData);
  });
}

// Usage
const fileInput = document.querySelector<HTMLInputElement>("#file-input")!;
const file = fileInput.files![0];

uploadFileWithProgress(file, (percent) => {
  console.log(`Upload progress: ${percent.toFixed(2)}%`);
})
  .then((response) => {
    console.log("Upload complete:", response);
  })
  .catch((error) => {
    console.error("Upload error:", error);
  });
```

---

## 6. ReactJS

### 6.1 Lifecycle

#### Khái niệm

**React Component Lifecycle** (Class Components - legacy):

1. **Mounting**: componentDidMount()
2. **Updating**: componentDidUpdate()
3. **Unmounting**: componentWillUnmount()

**Functional Components** (hiện đại): Sử dụng **Hooks** thay vì lifecycle methods.

#### Ví dụ

```typescript
// ========== CLASS COMPONENT LIFECYCLE (Legacy) ==========
import React, { Component } from "react";

interface State {
  count: number;
  data: any[];
}

class LifecycleExample extends Component<{}, State> {
  timer: number | undefined;

  constructor(props: {}) {
    super(props);
    this.state = {
      count: 0,
      data: [],
    };
    console.log("1. Constructor");
  }

  componentDidMount() {
    console.log("3. componentDidMount - Component mounted");
    // Fetch data, setup subscriptions
    this.fetchData();
    this.timer = window.setInterval(() => {
      this.setState({ count: this.state.count + 1 });
    }, 1000);
  }

  componentDidUpdate(prevProps: {}, prevState: State) {
    console.log("4. componentDidUpdate");
    if (prevState.count !== this.state.count) {
      console.log(`Count changed: ${prevState.count} -> ${this.state.count}`);
    }
  }

  componentWillUnmount() {
    console.log("5. componentWillUnmount - Cleanup");
    // Clear timers, cancel requests
    if (this.timer) {
      clearInterval(this.timer);
    }
  }

  async fetchData() {
    const response = await fetch("/api/data");
    const data = await response.json();
    this.setState({ data });
  }

  render() {
    console.log("2. Render");
    return (
      <div>
        <h1>Count: {this.state.count}</h1>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}

// ========== FUNCTIONAL COMPONENT WITH HOOKS (Modern) ==========
import React, { useState, useEffect } from "react";

function LifecycleHooks() {
  const [count, setCount] = useState(0);
  const [data, setData] = useState<any[]>([]);

  // componentDidMount + componentDidUpdate
  useEffect(() => {
    console.log("Component mounted or count changed");
    document.title = `Count: ${count}`;
  }, [count]); // Dependency array

  // componentDidMount (only once)
  useEffect(() => {
    console.log("Component mounted");
    fetchData();
  }, []); // Empty dependency array

  // componentWillUnmount
  useEffect(() => {
    const timer = setInterval(() => {
      setCount((c) => c + 1);
    }, 1000);

    return () => {
      console.log("Cleanup");
      clearInterval(timer);
    };
  }, []);

  async function fetchData() {
    const response = await fetch("/api/data");
    const data = await response.json();
    setData(data);
  }

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 6.2 Hook (useState, useEffect)

#### Khái niệm

**Hooks** là functions cho phép sử dụng state và lifecycle trong functional components.

**Built-in Hooks:**

- `useState`: State management
- `useEffect`: Side effects
- `useContext`: Context API
- `useReducer`: Complex state logic
- `useCallback`: Memoize functions
- `useMemo`: Memoize values
- `useRef`: DOM refs và mutable values

#### Ví dụ

```tsx
import React, {
  useState,
  useEffect,
  useRef,
  useContext,
  useMemo,
  useCallback,
} from "react";

// ========== useState ==========
function Counter() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("");
  const [user, setUser] = useState<User | null>(null);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>

      {/* Functional update (recommended) */}
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>

      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter name"
      />
    </div>
  );
}

// ========== useEffect ==========
function DataFetcher({ userId }: { userId: number }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // Effect function
    let cancelled = false;

    async function fetchUser() {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();

        if (!cancelled) {
          setUser(data);
          setError(null);
        }
      } catch (err) {
        if (!cancelled) {
          setError("Failed to fetch user");
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    }

    fetchUser();

    // Cleanup function
    return () => {
      cancelled = true;
    };
  }, [userId]); // Re-run when userId changes

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return null;

  return <div>User: {user.name}</div>;
}

// ========== useRef ==========
function InputFocus() {
  const inputRef = useRef<HTMLInputElement>(null);
  const renderCount = useRef(0);

  // Increment render count (doesn't cause re-render)
  renderCount.current++;

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
      <p>Render count: {renderCount.current}</p>
    </div>
  );
}

// ========== useMemo ==========
function ExpensiveCalculation({ numbers }: { numbers: number[] }) {
  // Expensive calculation only runs when numbers change
  const sum = useMemo(() => {
    console.log("Calculating sum...");
    return numbers.reduce((a, b) => a + b, 0);
  }, [numbers]);

  const average = useMemo(() => {
    console.log("Calculating average...");
    return sum / numbers.length;
  }, [sum, numbers.length]);

  return (
    <div>
      <p>Sum: {sum}</p>
      <p>Average: {average}</p>
    </div>
  );
}

// ========== useCallback ==========
function SearchableList() {
  const [query, setQuery] = useState("");
  const [items] = useState(["Apple", "Banana", "Cherry"]);

  // Memoized callback
  const handleSearch = useCallback((value: string) => {
    console.log("Searching for:", value);
    setQuery(value);
  }, []); // Dependencies empty - callback never changes

  const filteredItems = items.filter((item) =>
    item.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <div>
      <SearchInput onSearch={handleSearch} />
      <ul>
        {filteredItems.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

// Child component (won't re-render if onSearch doesn't change)
const SearchInput = React.memo(
  ({ onSearch }: { onSearch: (value: string) => void }) => {
    console.log("SearchInput rendered");
    return <input onChange={(e) => onSearch(e.target.value)} />;
  }
);

// ========== useContext ==========
interface ThemeContextType {
  theme: "light" | "dark";
  toggleTheme: () => void;
}

const ThemeContext = React.createContext<ThemeContextType | undefined>(
  undefined
);

function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<"light" | "dark">("light");

  const toggleTheme = () => {
    setTheme((t) => (t === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemedButton() {
  const context = useContext(ThemeContext);

  if (!context) {
    throw new Error("useContext must be used within ThemeProvider");
  }

  const { theme, toggleTheme } = context;

  return (
    <button
      onClick={toggleTheme}
      style={{ background: theme === "light" ? "#fff" : "#333" }}
    >
      Toggle Theme (Current: {theme})
    </button>
  );
}
```

### 6.3 Virtual DOM

#### Khái niệm

**Virtual DOM** là representation của Real DOM trong memory.

**Cách hoạt động:**

1. State thay đổi
2. React tạo new Virtual DOM tree
3. React so sánh (diffing) với previous Virtual DOM
4. React tính toán minimal changes
5. React cập nhật Real DOM (reconciliation)

**Lợi ích:**

- ✅ Performance: Chỉ update phần thay đổi
- ✅ Batching: Gom nhiều updates lại
- ✅ Declarative: Developer chỉ cần khai báo UI

#### Ví dụ

```tsx
// ========== VIRTUAL DOM DEMO ==========
function CounterDemo() {
  const [count, setCount] = useState(0);

  console.log("Component re-renders");
  // Nhưng chỉ có phần count update trong DOM!

  return (
    <div>
      <h1>Static Header</h1>
      <p>Count: {count}</p> {/* Chỉ phần này update */}
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <footer>Static Footer</footer>
    </div>
  );
}

// ========== RECONCILIATION EXAMPLE ==========
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: "Learn React" },
    { id: 2, text: "Build App" },
  ]);

  const addTodo = () => {
    setTodos([...todos, { id: Date.now(), text: "New Todo" }]);
    // React chỉ thêm 1 <li> mới, không re-render toàn bộ list
  };

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
        // Key helps React identify which items changed
      ))}
      <button onClick={addTodo}>Add Todo</button>
    </ul>
  );
}

// ========== WHY KEYS MATTER ==========
// ❌ BAD: Using index as key
function BadList() {
  const [items, setItems] = useState(["A", "B", "C"]);

  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
        // Problem: If you reorder items, React gets confused
      ))}
    </ul>
  );
}

// ✅ GOOD: Using unique IDs
function GoodList() {
  const [items, setItems] = useState([
    { id: 1, name: "A" },
    { id: 2, name: "B" },
    { id: 3, name: "C" },
  ]);

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
        // React can track items correctly even if reordered
      ))}
    </ul>
  );
}
```

### 6.4 React Router

#### Khái niệm

**React Router** là thư viện routing cho React applications.

**Main Components:**

- `BrowserRouter`: Router cho browser
- `Routes`: Container cho routes
- `Route`: Định nghĩa một route
- `Link`: Navigation link
- `Navigate`: Programmatic navigation
- `useNavigate`: Hook để navigate
- `useParams`: Hook để lấy URL params
- `useLocation`: Hook để lấy location info

#### Ví dụ

```tsx
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  useNavigate,
  useParams,
  useLocation,
} from "react-router-dom";

// ========== BASIC ROUTING ==========
function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
        <Link to="/contact">Contact</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:id" element={<UserDetail />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// ========== BASIC COMPONENTS ==========
function Home() {
  return <h1>Home Page</h1>;
}

function About() {
  return <h1>About Page</h1>;
}

function NotFound() {
  return <h1>404 - Page Not Found</h1>;
}

// ========== URL PARAMETERS ==========
function UserDetail() {
  const { id } = useParams<{ id: string }>();
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetch(`/api/users/${id}`)
      .then((res) => res.json())
      .then((data) => setUser(data));
  }, [id]);

  if (!user) return <div>Loading...</div>;

  return (
    <div>
      <h1>User: {user.name}</h1>
      <p>Email: {user.email}</p>
    </div>
  );
}

// ========== PROGRAMMATIC NAVIGATION ==========
function Login() {
  const navigate = useNavigate();
  const [credentials, setCredentials] = useState({ email: "", password: "" });

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    try {
      const response = await fetch("/api/login", {
        method: "POST",
        body: JSON.stringify(credentials),
      });

      if (response.ok) {
        // Navigate to dashboard after successful login
        navigate("/dashboard");
        // Or with state:
        // navigate('/dashboard', { state: { from: 'login' } });
      }
    } catch (error) {
      console.error("Login failed:", error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={credentials.email}
        onChange={(e) =>
          setCredentials({ ...credentials, email: e.target.value })
        }
      />
      <input
        type="password"
        value={credentials.password}
        onChange={(e) =>
          setCredentials({ ...credentials, password: e.target.value })
        }
      />
      <button type="submit">Login</button>
    </form>
  );
}

// ========== NESTED ROUTES ==========
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="profile">Profile</Link>
        <Link to="settings">Settings</Link>
      </nav>

      <Routes>
        <Route path="profile" element={<Profile />} />
        <Route path="settings" element={<Settings />} />
      </Routes>
    </div>
  );
}

// ========== PROTECTED ROUTE ==========
function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isAuthenticated = localStorage.getItem("token");

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return <>{children}</>;
}

// Usage
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>;

// ========== useLocation HOOK ==========
function Header() {
  const location = useLocation();

  return (
    <header>
      <p>Current path: {location.pathname}</p>
      <p>Search params: {location.search}</p>
      <p>Hash: {location.hash}</p>
    </header>
  );
}
```

---

## Bài tập thực hành

### Bài 1: API Integration

Tạo một UserManager component:

- Fetch và hiển thị danh sách users
- Thêm user mới
- Sửa user
- Xóa user
- Xử lý loading và error states

### Bài 2: Form với File Upload

Tạo form upload avatar:

- Input fields cho name, email
- File input cho avatar
- Progress bar khi upload
- Preview ảnh trước khi upload

### Bài 3: React App với Router

Tạo blog app với:

- Home page (danh sách posts)
- Post detail page (/:id)
- Create post page
- Profile page (protected route)
- 404 page

---

## Tài liệu tham khảo

1. [React Official Documentation](https://react.dev/)
2. [React Router Documentation](https://reactrouter.com/)
3. [MDN Web Docs - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
4. [RESTful API Design Best Practices](https://restfulapi.net/)

---

**Previous Module:** [← TypeScript](./03-typescript.md)  
**Next Module:** [GraphQL →](./05-graphql.md)

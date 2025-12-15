# Module 5: GraphQL

## Module Objectives

This module helps you understand GraphQL - một query language hiện đại cho APIs, cung cấp cách hiệu quả hơn để fetch dữ liệu so với REST API.

---

## 1. Understand GraphQL

### Concept

**GraphQL** là một query language và runtime cho APIs, được phát triển bởi Facebook năm 2012.

**Đặc điểm chính:**

- **Single Endpoint**: Chỉ một endpoint (thường là `/graphql`)
- **Declarative Data Fetching**: Client quyết định data cần lấy
- **Strong Typing**: Schema-based với type system
- **No Over/Under-fetching**: Lấy đúng data cần thiết

**Core Concepts:**

1. **Schema**: Định nghĩa types và relationships
2. **Queries**: Đọc data (GET)
3. **Mutations**: Thay đổi data (POST, PUT, DELETE)
4. **Subscriptions**: Real-time updates
5. **Resolvers**: Functions trả về data

### Examples

```graphql
# ========== SCHEMA DEFINITION ==========
type User {
  id: ID!
  name: String!
  email: String!
  age: Int
  posts: [Post!]!
  createdAt: DateTime!
}

type Post {
  id: ID!
  title: String!
  content: String!
  published: Boolean!
  author: User!
  comments: [Comment!]!
}

type Comment {
  id: ID!
  text: String!
  author: User!
  post: Post!
}

# Root Query Type
type Query {
  # Get single user
  user(id: ID!): User

  # Get all users
  users(limit: Int, offset: Int): [User!]!

  # Get single post
  post(id: ID!): Post

  # Search posts
  searchPosts(query: String!): [Post!]!
}

# Root Mutation Type
type Mutation {
  # Create user
  createUser(name: String!, email: String!, age: Int): User!

  # Update user
  updateUser(id: ID!, name: String, email: String, age: Int): User!

  # Delete user
  deleteUser(id: ID!): Boolean!

  # Create post
  createPost(title: String!, content: String!, authorId: ID!): Post!
}

# Root Subscription Type
type Subscription {
  # Subscribe to new posts
  postCreated: Post!

  # Subscribe to comments on a post
  commentAdded(postId: ID!): Comment!
}

# ========== EXAMPLE QUERIES ==========

# Query 1: Get user with specific fields
query GetUser {
  user(id: "1") {
    id
    name
    email
  }
}

# Response:
{
  "data": {
    "user": {
      "id": "1",
      "name": "Alice",
      "email": "alice@example.com"
    }
  }
}

# Query 2: Get user with nested data
query GetUserWithPosts {
  user(id: "1") {
    id
    name
    posts {
      id
      title
      comments {
        id
        text
      }
    }
  }
}

# Query 3: Multiple queries in one request
query GetMultipleData {
  user1: user(id: "1") {
    name
    email
  }
  user2: user(id: "2") {
    name
    email
  }
  allPosts: searchPosts(query: "React") {
    title
    author {
      name
    }
  }
}

# ========== EXAMPLE MUTATIONS ==========

# Mutation 1: Create user
mutation CreateUser {
  createUser(
    name: "Bob"
    email: "bob@example.com"
    age: 30
  ) {
    id
    name
    email
    createdAt
  }
}

# Mutation 2: Update user
mutation UpdateUser {
  updateUser(
    id: "1"
    name: "Alice Updated"
    age: 26
  ) {
    id
    name
    age
  }
}

# Mutation 3: Create post
mutation CreatePost {
  createPost(
    title: "My First Post"
    content: "This is the content"
    authorId: "1"
  ) {
    id
    title
    author {
      name
    }
  }
}

# ========== VARIABLES ==========
query GetUserById($userId: ID!) {
  user(id: $userId) {
    id
    name
    email
  }
}

# Variables:
{
  "userId": "1"
}

# ========== FRAGMENTS ==========
fragment UserInfo on User {
  id
  name
  email
}

query GetUsers {
  user1: user(id: "1") {
    ...UserInfo
  }
  user2: user(id: "2") {
    ...UserInfo
  }
}
```

---

## 2. Why GraphQL? (What is different between GraphQL vs RESTful API)

### So sánh GraphQL vs REST

| Feature            | GraphQL                       | REST                            |
| ------------------ | ----------------------------- | ------------------------------- |
| **Endpoints**      | Single endpoint               | Multiple endpoints              |
| **Data Fetching**  | Request exactly what you need | Fixed response structure        |
| **Over-fetching**  | ❌ No                         | ✅ Yes (get unnecessary data)   |
| **Under-fetching** | ❌ No                         | ✅ Yes (need multiple requests) |
| **Versioning**     | No need (schema evolution)    | API versioning (v1, v2)         |
| **Learning Curve** | Steeper                       | Easier                          |
| **Caching**        | More complex                  | Simpler (HTTP caching)          |
| **Type System**    | Strong typing                 | No built-in typing              |
| **Real-time**      | Subscriptions built-in        | Requires WebSockets/SSE         |

### Examples so sánh

```typescript
// ========== REST API ==========

// Problem 1: Multiple Requests (Under-fetching)
// Get user
GET /api/users/1
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}

// Get user's posts
GET /api/users/1/posts
[
  { "id": 1, "title": "Post 1" },
  { "id": 2, "title": "Post 2" }
]

// Get post comments
GET /api/posts/1/comments
[...]

// Total: 3 requests!

// Problem 2: Over-fetching
GET /api/users/1
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com",
  "age": 25,
  "address": {...},        // Don't need
  "phoneNumber": "...",    // Don't need
  "preferences": {...},    // Don't need
  "createdAt": "...",      // Don't need
  // ... lots of unnecessary data
}

// ========== GRAPHQL ==========

// Solution: Single Request with exact data needed
query GetUserWithPosts {
  user(id: "1") {
    name
    email
    posts {
      title
      comments {
        text
        author {
          name
        }
      }
    }
  }
}

// Response: Exactly what we asked for!
{
  "data": {
    "user": {
      "name": "Alice",
      "email": "alice@example.com",
      "posts": [
        {
          "title": "Post 1",
          "comments": [
            { "text": "Great post!", "author": { "name": "Bob" } }
          ]
        }
      ]
    }
  }
}

// ========== CODE COMPARISON ==========

// REST API - Multiple requests
async function getUserData(userId) {
  const user = await fetch(`/api/users/${userId}`).then(r => r.json());
  const posts = await fetch(`/api/users/${userId}/posts`).then(r => r.json());
  const comments = await Promise.all(
    posts.map(post =>
      fetch(`/api/posts/${post.id}/comments`).then(r => r.json())
    )
  );

  return { user, posts, comments };
}

// GraphQL - Single request
async function getUserData(userId) {
  const query = `
    query GetUser($id: ID!) {
      user(id: $id) {
        name
        email
        posts {
          title
          comments {
            text
            author { name }
          }
        }
      }
    }
  `;

  const response = await fetch('/graphql', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query, variables: { id: userId } })
  });

  const { data } = await response.json();
  return data.user;
}
```

### Khi nào dùng GraphQL?

**✅ Sử dụng GraphQL khi:**

- App cần fetch nhiều related data
- Có nhiều clients khác nhau (mobile, web, desktop)
- Cần real-time updates
- Muốn tránh over/under-fetching
- Team lớn, cần strong typing

**❌ Không nên dùng GraphQL khi:**

- Simple CRUD app
- File uploads (better with REST)
- Caching requirements phức tạp
- Team nhỏ, simple requirements

---

## 3. GraphQL concept (useQuery, useMutation, useLazyQuery)

### Concept

**Apollo Client** là GraphQL client phổ biến nhất cho React.

**Main Hooks:**

- `useQuery`: Fetch data (tự động fetch khi component mount)
- `useLazyQuery`: Fetch data manually (gọi khi cần)
- `useMutation`: Modify data
- `useSubscription`: Real-time updates

### Examples Setup

```typescript
// ========== SETUP APOLLO CLIENT ==========
import {
  ApolloClient,
  InMemoryCache,
  ApolloProvider,
  HttpLink,
} from "@apollo/client";

// Create Apollo Client
const client = new ApolloClient({
  link: new HttpLink({
    uri: "https://api.example.com/graphql",
    headers: {
      authorization: `Bearer ${localStorage.getItem("token")}`,
    },
  }),
  cache: new InMemoryCache(),
});

// Wrap app with ApolloProvider
function App() {
  return (
    <ApolloProvider client={client}>
      <MyApp />
    </ApolloProvider>
  );
}
```

### useQuery

```typescript
import { useQuery, gql } from "@apollo/client";

// ========== DEFINE QUERY ==========
const GET_USERS = gql`
  query GetUsers {
    users {
      id
      name
      email
    }
  }
`;

// ========== USE QUERY ==========
function UserList() {
  const { loading, error, data, refetch } = useQuery(GET_USERS);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <button onClick={() => refetch()}>Refresh</button>
      <ul>
        {data.users.map((user) => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

// ========== QUERY WITH VARIABLES ==========
const GET_USER_BY_ID = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      name
      email
      posts {
        id
        title
      }
    }
  }
`;

function UserDetail({ userId }: { userId: string }) {
  const { loading, error, data } = useQuery(GET_USER_BY_ID, {
    variables: { id: userId },
  });

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  const { user } = data;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
      <h2>Posts:</h2>
      <ul>
        {user.posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

// ========== QUERY WITH OPTIONS ==========
function UserListWithOptions() {
  const { loading, error, data } = useQuery(GET_USERS, {
    // Polling: Refetch every 5 seconds
    pollInterval: 5000,

    // Skip query
    skip: false,

    // Fetch policy
    fetchPolicy: "cache-first", // 'cache-first' | 'network-only' | 'cache-only' | 'no-cache'

    // On complete
    onCompleted: (data) => {
      console.log("Query completed:", data);
    },

    // On error
    onError: (error) => {
      console.error("Query error:", error);
    },
  });

  // ...
}
```

### useLazyQuery

```typescript
import { useLazyQuery, gql } from "@apollo/client";

const SEARCH_USERS = gql`
  query SearchUsers($query: String!) {
    searchUsers(query: $query) {
      id
      name
      email
    }
  }
`;

// ========== USE LAZY QUERY ==========
function UserSearch() {
  const [searchQuery, setSearchQuery] = useState("");

  // useLazyQuery returns [executeQuery, { loading, error, data }]
  const [searchUsers, { loading, error, data }] = useLazyQuery(SEARCH_USERS);

  const handleSearch = () => {
    searchUsers({ variables: { query: searchQuery } });
  };

  return (
    <div>
      <input
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
        placeholder="Search users..."
      />
      <button onClick={handleSearch}>Search</button>

      {loading && <div>Searching...</div>}
      {error && <div>Error: {error.message}</div>}
      {data && (
        <ul>
          {data.searchUsers.map((user) => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      )}
    </div>
  );
}

// ========== LAZY QUERY WITH CALLBACK ==========
function ConditionalDataFetch() {
  const [getUser, { loading, data }] = useLazyQuery(GET_USER_BY_ID, {
    onCompleted: (data) => {
      console.log("User loaded:", data.user);
    },
  });

  const loadUser = (userId: string) => {
    getUser({ variables: { id: userId } });
  };

  return (
    <div>
      <button onClick={() => loadUser("1")}>Load User 1</button>
      <button onClick={() => loadUser("2")}>Load User 2</button>

      {loading && <div>Loading...</div>}
      {data && <div>User: {data.user.name}</div>}
    </div>
  );
}
```

### useMutation

```typescript
import { useMutation, gql } from "@apollo/client";

// ========== DEFINE MUTATION ==========
const CREATE_USER = gql`
  mutation CreateUser($name: String!, $email: String!) {
    createUser(name: $name, email: $email) {
      id
      name
      email
      createdAt
    }
  }
`;

const UPDATE_USER = gql`
  mutation UpdateUser($id: ID!, $name: String, $email: String) {
    updateUser(id: $id, name: $name, email: $email) {
      id
      name
      email
    }
  }
`;

const DELETE_USER = gql`
  mutation DeleteUser($id: ID!) {
    deleteUser(id: $id)
  }
`;

// ========== USE MUTATION ==========
function CreateUserForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const [createUser, { loading, error, data }] = useMutation(CREATE_USER, {
    // Refetch queries after mutation
    refetchQueries: [{ query: GET_USERS }],

    // Or update cache manually
    update(cache, { data: { createUser } }) {
      const existingUsers: any = cache.readQuery({ query: GET_USERS });
      cache.writeQuery({
        query: GET_USERS,
        data: {
          users: [...existingUsers.users, createUser],
        },
      });
    },

    // On complete
    onCompleted: (data) => {
      console.log("User created:", data.createUser);
      setName("");
      setEmail("");
    },

    // On error
    onError: (error) => {
      console.error("Error creating user:", error);
    },
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    createUser({ variables: { name, email } });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
        required
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
        required
      />
      <button type="submit" disabled={loading}>
        {loading ? "Creating..." : "Create User"}
      </button>

      {error && <div>Error: {error.message}</div>}
      {data && <div>User created: {data.createUser.name}</div>}
    </form>
  );
}

// ========== UPDATE MUTATION ==========
function UpdateUserForm({ user }: { user: User }) {
  const [name, setName] = useState(user.name);
  const [email, setEmail] = useState(user.email);

  const [updateUser, { loading }] = useMutation(UPDATE_USER, {
    onCompleted: () => {
      alert("User updated successfully!");
    },
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    updateUser({
      variables: {
        id: user.id,
        name,
        email,
      },
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <input value={email} onChange={(e) => setEmail(e.target.value)} />
      <button type="submit" disabled={loading}>
        {loading ? "Updating..." : "Update"}
      </button>
    </form>
  );
}

// ========== DELETE MUTATION ==========
function DeleteUserButton({ userId }: { userId: string }) {
  const [deleteUser, { loading }] = useMutation(DELETE_USER, {
    variables: { id: userId },

    // Update cache after delete
    update(cache) {
      cache.modify({
        fields: {
          users(existingUsers = [], { readField }) {
            return existingUsers.filter(
              (userRef: any) => userId !== readField("id", userRef)
            );
          },
        },
      });
    },

    onCompleted: () => {
      alert("User deleted!");
    },
  });

  return (
    <button onClick={() => deleteUser()} disabled={loading}>
      {loading ? "Deleting..." : "Delete"}
    </button>
  );
}
```

---

## 4. Advance

### 4.1 Caching in GraphQL (Fetch Policy)

#### Concept

Apollo Client tự động cache data theo `id` và `__typename`.

**Fetch Policies:**

- `cache-first` (default): Kiểm tra cache trước, fetch nếu không có
- `cache-only`: Chỉ dùng cache, không fetch
- `network-only`: Luôn fetch, update cache
- `no-cache`: Fetch nhưng không cache
- `cache-and-network`: Dùng cache + fetch background

#### Examples

```typescript
import { useQuery, gql } from "@apollo/client";

const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      name
      email
    }
  }
`;

// ========== CACHE-FIRST (Default) ==========
function UserProfile({ userId }: { userId: string }) {
  const { data } = useQuery(GET_USER, {
    variables: { id: userId },
    fetchPolicy: "cache-first",
    // First check cache, if not found, fetch from network
  });

  return <div>{data?.user.name}</div>;
}

// ========== NETWORK-ONLY ==========
function LiveUserProfile({ userId }: { userId: string }) {
  const { data } = useQuery(GET_USER, {
    variables: { id: userId },
    fetchPolicy: "network-only",
    // Always fetch fresh data
  });

  return <div>{data?.user.name}</div>;
}

// ========== CACHE-AND-NETWORK ==========
function UserProfileWithStaleData({ userId }: { userId: string }) {
  const { data, loading } = useQuery(GET_USER, {
    variables: { id: userId },
    fetchPolicy: "cache-and-network",
    // Show cached data immediately, fetch in background
  });

  return (
    <div>
      {data?.user.name}
      {loading && <span> (updating...)</span>}
    </div>
  );
}

// ========== MANUAL CACHE UPDATE ==========
import { useApolloClient } from "@apollo/client";

function ManualCacheUpdate() {
  const client = useApolloClient();

  const updateCache = () => {
    // Write to cache
    client.writeQuery({
      query: GET_USER,
      variables: { id: "1" },
      data: {
        user: {
          __typename: "User",
          id: "1",
          name: "Updated Name",
          email: "updated@example.com",
        },
      },
    });
  };

  const readCache = () => {
    // Read from cache
    const data = client.readQuery({
      query: GET_USER,
      variables: { id: "1" },
    });
    console.log(data);
  };

  const clearCache = () => {
    // Clear all cache
    client.clearStore();

    // Or reset store
    // client.resetStore();
  };

  return (
    <div>
      <button onClick={updateCache}>Update Cache</button>
      <button onClick={readCache}>Read Cache</button>
      <button onClick={clearCache}>Clear Cache</button>
    </div>
  );
}
```

### 4.2 Fragment

#### Concept

**Fragments** cho phép tái sử dụng query fields.

#### Examples

```typescript
import { gql } from "@apollo/client";

// ========== DEFINE FRAGMENTS ==========
const USER_FRAGMENT = gql`
  fragment UserInfo on User {
    id
    name
    email
  }
`;

const POST_FRAGMENT = gql`
  fragment PostInfo on Post {
    id
    title
    content
    createdAt
  }
`;

const USER_WITH_POSTS_FRAGMENT = gql`
  fragment UserWithPosts on User {
    ...UserInfo
    posts {
      ...PostInfo
    }
  }
  ${USER_FRAGMENT}
  ${POST_FRAGMENT}
`;

// ========== USE FRAGMENTS IN QUERIES ==========
const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

const GET_USER_WITH_POSTS = gql`
  query GetUserWithPosts($id: ID!) {
    user(id: $id) {
      ...UserWithPosts
    }
  }
  ${USER_WITH_POSTS_FRAGMENT}
`;

const GET_MULTIPLE_USERS = gql`
  query GetMultipleUsers {
    user1: user(id: "1") {
      ...UserInfo
    }
    user2: user(id: "2") {
      ...UserInfo
    }
    allUsers: users {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

// ========== FRAGMENTS IN MUTATIONS ==========
const CREATE_USER = gql`
  mutation CreateUser($name: String!, $email: String!) {
    createUser(name: $name, email: $email) {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

// ========== NESTED FRAGMENTS ==========
const COMMENT_FRAGMENT = gql`
  fragment CommentInfo on Comment {
    id
    text
    author {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

const POST_WITH_COMMENTS_FRAGMENT = gql`
  fragment PostWithComments on Post {
    ...PostInfo
    author {
      ...UserInfo
    }
    comments {
      ...CommentInfo
    }
  }
  ${POST_FRAGMENT}
  ${USER_FRAGMENT}
  ${COMMENT_FRAGMENT}
`;

// Usage
function UserWithPosts({ userId }: { userId: string }) {
  const { data } = useQuery(GET_USER_WITH_POSTS, {
    variables: { id: userId },
  });

  return (
    <div>
      <h1>{data?.user.name}</h1>
      <p>{data?.user.email}</p>
      <ul>
        {data?.user.posts.map((post: any) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

---

## Tổng hợp: Complete Example

```typescript
// ========== COMPLETE GRAPHQL APP ==========
import React, { useState } from "react";
import {
  ApolloClient,
  InMemoryCache,
  ApolloProvider,
  useQuery,
  useMutation,
  useLazyQuery,
  gql,
} from "@apollo/client";

// Setup Apollo Client
const client = new ApolloClient({
  uri: "https://api.example.com/graphql",
  cache: new InMemoryCache(),
});

// Fragments
const USER_FRAGMENT = gql`
  fragment UserInfo on User {
    id
    name
    email
  }
`;

// Queries
const GET_USERS = gql`
  query GetUsers {
    users {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

const SEARCH_USERS = gql`
  query SearchUsers($query: String!) {
    searchUsers(query: $query) {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

// Mutations
const CREATE_USER = gql`
  mutation CreateUser($name: String!, $email: String!) {
    createUser(name: $name, email: $email) {
      ...UserInfo
    }
  }
  ${USER_FRAGMENT}
`;

const DELETE_USER = gql`
  mutation DeleteUser($id: ID!) {
    deleteUser(id: $id)
  }
`;

// Components
function UserManager() {
  const { loading, error, data, refetch } = useQuery(GET_USERS);
  const [createUser] = useMutation(CREATE_USER, {
    refetchQueries: [{ query: GET_USERS }],
  });
  const [deleteUser] = useMutation(DELETE_USER, {
    refetchQueries: [{ query: GET_USERS }],
  });

  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const handleCreate = async (e: React.FormEvent) => {
    e.preventDefault();
    await createUser({ variables: { name, email } });
    setName("");
    setEmail("");
  };

  const handleDelete = (id: string) => {
    deleteUser({ variables: { id } });
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>User Manager</h1>

      <form onSubmit={handleCreate}>
        <input
          value={name}
          onChange={(e) => setName(e.target.value)}
          placeholder="Name"
          required
        />
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Email"
          required
        />
        <button type="submit">Create User</button>
      </form>

      <button onClick={() => refetch()}>Refresh</button>

      <ul>
        {data.users.map((user: any) => (
          <li key={user.id}>
            {user.name} - {user.email}
            <button onClick={() => handleDelete(user.id)}>Delete</button>
          </li>
        ))}
      </ul>

      <UserSearch />
    </div>
  );
}

function UserSearch() {
  const [query, setQuery] = useState("");
  const [search, { loading, data }] = useLazyQuery(SEARCH_USERS);

  const handleSearch = () => {
    search({ variables: { query } });
  };

  return (
    <div>
      <h2>Search Users</h2>
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search..."
      />
      <button onClick={handleSearch}>Search</button>

      {loading && <div>Searching...</div>}
      {data && (
        <ul>
          {data.searchUsers.map((user: any) => (
            <li key={user.id}>
              {user.name} - {user.email}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}

// App
function App() {
  return (
    <ApolloProvider client={client}>
      <UserManager />
    </ApolloProvider>
  );
}

export default App;
```

---

## Practice Exercises

### Bài 1: Basic GraphQL Query

Tạo component hiển thị danh sách posts với:

- Title, content, author name
- Loading state
- Error handling
- Refresh button

### Bài 2: GraphQL Mutations

Tạo blog post manager với:

- Create new post
- Update post
- Delete post
- Auto-refresh sau mỗi mutation

### Bài 3: Search với LazyQuery

Tạo search feature:

- Input search
- Lazy query khi click Search
- Hiển thị kết quả
- Clear results

### Bài 4: Advanced với Fragments

Tạo user profile page với:

- User info fragment
- Posts fragment
- Nested author trong posts
- Reuse fragments trong nhiều queries

---

## References

1. [GraphQL Official Documentation](https://graphql.org/learn/)
2. [Apollo Client Documentation](https://www.apollographql.com/docs/react/)
3. [GraphQL vs REST](https://www.apollographql.com/blog/graphql-vs-rest-5d425123e34b)
   4 [How to GraphQL](https://www.howtographql.com/)

---

**Previous Module:** [← ReactJS + Restful API](./04-reactjs-restful-api.md)  
**Next Module:** [Clean Code and Design Pattern →](./06-clean-code-design-pattern.md)

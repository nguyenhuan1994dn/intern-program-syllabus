# Module 2: HTML, CSS

## Module Objectives

This module helps you master modern layout techniques, responsive CSS, and semantic HTML to build professional web interfaces.

---

## 1. Flexbox

### Concept

**Flexbox** (Flexible Box Layout) là một CSS layout module giúp sắp xếp items trong container một cách linh hoạt theo một chiều (row hoặc column).

**Main concepts:**

- **Flex Container**: Element cha có `display: flex`
- **Flex Items**: Các elements con
- **Main Axis**: Trục chính (mặc định là horizontal)
- **Cross Axis**: Trục phụ (vuông góc với main axis)

**Flex Container Properties:**

- `flex-direction`: Hướng của main axis (row, column, row-reverse, column-reverse)
- `justify-content`: Căn chỉnh theo main axis
- `align-items`: Căn chỉnh theo cross axis
- `flex-wrap`: Cho phép items xuống dòng
- `gap`: Khoảng cách giữa items

**Flex Item Properties:**

- `flex-grow`: Khả năng mở rộng
- `flex-shrink`: Khả năng thu nhỏ
- `flex-basis`: Kích thước ban đầu
- `flex`: Shorthand (grow, shrink, basis)
- `align-self`: Override align-items

### Examples

#### HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flexbox Example</title>
    <link rel="stylesheet" href="flexbox.css" />
  </head>
  <body>
    <!-- Basic Flexbox Container -->
    <div class="flex-container">
      <div class="flex-item">Item 1</div>
      <div class="flex-item">Item 2</div>
      <div class="flex-item">Item 3</div>
    </div>

    <!-- Navigation Bar with Flexbox -->
    <nav class="navbar">
      <div class="logo">MyBrand</div>
      <ul class="nav-links">
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
      <button class="btn-login">Login</button>
    </nav>

    <!-- Card Layout with Flexbox -->
    <div class="card-container">
      <div class="card">
        <img src="image1.jpg" alt="Product 1" />
        <h3>Product 1</h3>
        <p>Description of product 1</p>
        <button>Buy Now</button>
      </div>
      <div class="card">
        <img src="image2.jpg" alt="Product 2" />
        <h3>Product 2</h3>
        <p>Description of product 2</p>
        <button>Buy Now</button>
      </div>
      <div class="card">
        <img src="image3.jpg" alt="Product 3" />
        <h3>Product 3</h3>
        <p>Description of product 3</p>
        <button>Buy Now</button>
      </div>
    </div>

    <!-- Centering with Flexbox -->
    <div class="center-container">
      <div class="centered-content">
        <h1>Perfectly Centered</h1>
        <p>Both horizontally and vertically</p>
      </div>
    </div>
  </body>
</html>
```

#### CSS

```css
/* Basic Flexbox Container */
.flex-container {
  display: flex;
  justify-content: space-between; /* Căn đều items */
  align-items: center; /* Căn giữa theo chiều dọc */
  gap: 20px; /* Khoảng cách giữa items */
  padding: 20px;
  background-color: #f0f0f0;
}

.flex-item {
  background-color: #4caf50;
  color: white;
  padding: 20px;
  text-align: center;
  flex: 1; /* Chia đều không gian */
}

.flex-item:nth-child(2) {
  flex: 2; /* Item 2 rộng gấp đôi */
}

/* Navigation Bar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #333;
  color: white;
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: white;
  text-decoration: none;
  transition: color 0.3s;
}

.nav-links a:hover {
  color: #4caf50;
}

.btn-login {
  padding: 0.5rem 1.5rem;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* Card Layout */
.card-container {
  display: flex;
  flex-wrap: wrap; /* Cho phép xuống dòng */
  gap: 20px;
  padding: 20px;
}

.card {
  display: flex;
  flex-direction: column; /* Sắp xếp theo chiều dọc */
  flex: 1 1 300px; /* grow shrink basis */
  min-width: 250px;
  max-width: 350px;
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card h3 {
  padding: 1rem;
  margin: 0;
}

.card p {
  padding: 0 1rem;
  flex-grow: 1; /* Chiếm hết không gian còn lại */
}

.card button {
  margin: 1rem;
  margin-top: auto; /* Đẩy button xuống dưới */
  padding: 0.75rem;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* Perfect Centering */
.center-container {
  display: flex;
  justify-content: center; /* Căn giữa horizontal */
  align-items: center; /* Căn giữa vertical */
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.centered-content {
  text-align: center;
  color: white;
}

/* Responsive Flexbox */
@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    gap: 1rem;
  }

  .nav-links {
    flex-direction: column;
    gap: 1rem;
  }

  .card-container {
    flex-direction: column;
  }
}
```

---

## 2. Grid Box

### Concept

**CSS Grid** là hệ thống layout hai chiều (rows và columns), mạnh mẽ hơn Flexbox cho các layouts phức tạp.

**Main concepts:**

- **Grid Container**: Element cha có `display: grid`
- **Grid Items**: Các elements con
- **Grid Lines**: Đường phân chia rows/columns
- **Grid Tracks**: Rows hoặc columns
- **Grid Cells**: Ô cơ bản
- **Grid Areas**: Vùng gồm nhiều cells

**Grid Container Properties:**

- `grid-template-columns`: Định nghĩa columns
- `grid-template-rows`: Định nghĩa rows
- `grid-template-areas`: Định nghĩa layout bằng tên
- `gap` / `grid-gap`: Khoảng cách giữa items
- `justify-items`, `align-items`: Căn chỉnh items
- `justify-content`, `align-content`: Căn chỉnh grid

**Grid Item Properties:**

- `grid-column`: Vị trí column (start/end)
- `grid-row`: Vị trí row (start/end)
- `grid-area`: Đặt tên hoặc vị trí
- `justify-self`, `align-self`: Căn chỉnh item

### Examples

#### HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS Grid Example</title>
    <link rel="stylesheet" href="grid.css" />
  </head>
  <body>
    <!-- Basic Grid -->
    <div class="grid-basic">
      <div class="grid-item">1</div>
      <div class="grid-item">2</div>
      <div class="grid-item">3</div>
      <div class="grid-item">4</div>
      <div class="grid-item">5</div>
      <div class="grid-item">6</div>
    </div>

    <!-- Page Layout with Grid Areas -->
    <div class="page-layout">
      <header class="header">Header</header>
      <aside class="sidebar">Sidebar</aside>
      <main class="main-content">Main Content</main>
      <aside class="ads">Ads</aside>
      <footer class="footer">Footer</footer>
    </div>

    <!-- Image Gallery -->
    <div class="gallery">
      <div class="gallery-item item-1">
        <img src="image1.jpg" alt="Image 1" />
      </div>
      <div class="gallery-item item-2">
        <img src="image2.jpg" alt="Image 2" />
      </div>
      <div class="gallery-item item-3">
        <img src="image3.jpg" alt="Image 3" />
      </div>
      <div class="gallery-item item-4">
        <img src="image4.jpg" alt="Image 4" />
      </div>
      <div class="gallery-item item-5">
        <img src="image5.jpg" alt="Image 5" />
      </div>
    </div>

    <!-- Responsive Grid -->
    <div class="product-grid">
      <div class="product">Product 1</div>
      <div class="product">Product 2</div>
      <div class="product">Product 3</div>
      <div class="product">Product 4</div>
      <div class="product">Product 5</div>
      <div class="product">Product 6</div>
    </div>
  </body>
</html>
```

#### CSS

```css
/* Basic Grid - 3 columns, auto rows */
.grid-basic {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 cột bằng nhau */
  grid-template-rows: auto;
  gap: 20px;
  padding: 20px;
}

.grid-item {
  background-color: #4caf50;
  color: white;
  padding: 40px;
  text-align: center;
  font-size: 2rem;
  border-radius: 8px;
}

/* Page Layout with Grid Areas */
.page-layout {
  display: grid;
  grid-template-columns: 200px 1fr 150px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "sidebar main ads"
    "footer footer footer";
  gap: 10px;
  min-height: 100vh;
  padding: 10px;
}

.header {
  grid-area: header;
  background-color: #333;
  color: white;
  padding: 20px;
  text-align: center;
}

.sidebar {
  grid-area: sidebar;
  background-color: #f4f4f4;
  padding: 20px;
}

.main-content {
  grid-area: main;
  background-color: white;
  padding: 20px;
}

.ads {
  grid-area: ads;
  background-color: #f4f4f4;
  padding: 20px;
}

.footer {
  grid-area: footer;
  background-color: #333;
  color: white;
  padding: 20px;
  text-align: center;
}

/* Image Gallery with different sizes */
.gallery {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(3, 200px);
  gap: 10px;
  padding: 20px;
}

.gallery-item {
  overflow: hidden;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s;
}

.gallery-item img:hover {
  transform: scale(1.1);
}

/* Spanning items */
.item-1 {
  grid-column: 1 / 3; /* Chiếm 2 columns */
  grid-row: 1 / 3; /* Chiếm 2 rows */
}

.item-2 {
  grid-column: 3 / 5;
}

.item-3 {
  grid-row: 2 / 4;
}

/* Responsive Grid - auto-fit */
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
}

.product {
  background-color: #2196f3;
  color: white;
  padding: 60px 20px;
  text-align: center;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Advanced Grid Functions */
.advanced-grid {
  display: grid;
  /* minmax: tối thiểu 200px, tối đa 1fr */
  grid-template-columns: repeat(3, minmax(200px, 1fr));
  /* fit-content: vừa đủ với content */
  grid-template-rows: fit-content(300px) auto;
  gap: 1rem;
}

/* Responsive Grid */
@media (max-width: 1024px) {
  .page-layout {
    grid-template-columns: 150px 1fr;
    grid-template-areas:
      "header header"
      "sidebar main"
      "ads ads"
      "footer footer";
  }
}

@media (max-width: 768px) {
  .grid-basic {
    grid-template-columns: repeat(2, 1fr); /* 2 cột */
  }

  .page-layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "ads"
      "footer";
  }

  .gallery {
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: auto;
  }

  .item-1,
  .item-2,
  .item-3 {
    grid-column: auto;
    grid-row: auto;
  }
}

@media (max-width: 480px) {
  .grid-basic {
    grid-template-columns: 1fr; /* 1 cột */
  }

  .gallery {
    grid-template-columns: 1fr;
  }
}
```

---

## 3. Layout

### Concept

Layout là cách sắp xếp elements trên trang web. Các kỹ thuật layout chính:

1. **Normal Flow**: Default layout (block và inline elements)
2. **Float**: Đưa element sang trái/phải (legacy, ít dùng)
3. **Position**: absolute, relative, fixed, sticky
4. **Flexbox**: Layout một chiều
5. **Grid**: Layout hai chiều
6. **Multi-column**: Chia nội dung thành nhiều cột

### Examples

#### HTML & CSS

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Layout Examples</title>
    <style>
      /* Reset */
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      /* Holy Grail Layout */
      body {
        min-height: 100vh;
        display: flex;
        flex-direction: column;
      }

      .header {
        background-color: #333;
        color: white;
        padding: 1rem;
        text-align: center;
      }

      .container {
        display: flex;
        flex: 1; /* Chiếm hết không gian còn lại */
      }

      .sidebar-left {
        width: 200px;
        background-color: #f4f4f4;
        padding: 1rem;
      }

      .main {
        flex: 1;
        padding: 1rem;
        background-color: white;
      }

      .sidebar-right {
        width: 200px;
        background-color: #f4f4f4;
        padding: 1rem;
      }

      .footer {
        background-color: #333;
        color: white;
        padding: 1rem;
        text-align: center;
      }

      /* Position Examples */
      .position-demo {
        position: relative;
        height: 400px;
        background-color: #e0e0e0;
        margin: 20px;
      }

      .static-box {
        position: static; /* Default */
        background-color: #4caf50;
        padding: 20px;
        margin: 10px;
      }

      .relative-box {
        position: relative;
        top: 20px;
        left: 30px;
        background-color: #2196f3;
        padding: 20px;
        margin: 10px;
      }

      .absolute-box {
        position: absolute;
        top: 50px;
        right: 50px;
        background-color: #f44336;
        padding: 20px;
        color: white;
      }

      .fixed-box {
        position: fixed;
        bottom: 20px;
        right: 20px;
        background-color: #ff9800;
        padding: 20px;
        color: white;
        border-radius: 50%;
        cursor: pointer;
      }

      .sticky-box {
        position: sticky;
        top: 0;
        background-color: #9c27b0;
        color: white;
        padding: 20px;
        z-index: 100;
      }

      /* Z-index Example */
      .stack-container {
        position: relative;
        height: 300px;
        margin: 20px;
      }

      .layer {
        position: absolute;
        width: 200px;
        height: 200px;
        padding: 20px;
        color: white;
      }

      .layer-1 {
        background-color: red;
        top: 0;
        left: 0;
        z-index: 1;
      }

      .layer-2 {
        background-color: green;
        top: 50px;
        left: 50px;
        z-index: 2;
      }

      .layer-3 {
        background-color: blue;
        top: 100px;
        left: 100px;
        z-index: 3;
      }

      /* Multi-column Layout */
      .article {
        column-count: 3;
        column-gap: 30px;
        column-rule: 2px solid #ddd;
        padding: 20px;
        text-align: justify;
      }

      .article h2 {
        column-span: all; /* Span across all columns */
        margin-bottom: 20px;
      }

      /* Responsive Layout */
      @media (max-width: 768px) {
        .container {
          flex-direction: column;
        }

        .sidebar-left,
        .sidebar-right {
          width: 100%;
        }

        .article {
          column-count: 1;
        }
      }
    </style>
  </head>
  <body>
    <!-- Holy Grail Layout -->
    <header class="header">
      <h1>Header</h1>
    </header>

    <div class="container">
      <aside class="sidebar-left">
        <h3>Left Sidebar</h3>
        <nav>
          <ul>
            <li><a href="#">Link 1</a></li>
            <li><a href="#">Link 2</a></li>
            <li><a href="#">Link 3</a></li>
          </ul>
        </nav>
      </aside>

      <main class="main">
        <h2>Main Content</h2>

        <!-- Sticky Element -->
        <div class="sticky-box">I'm Sticky! Scroll down to see me stick.</div>

        <p>Lorem ipsum dolor sit amet...</p>

        <!-- Position Demo -->
        <div class="position-demo">
          <div class="static-box">Static (Default)</div>
          <div class="relative-box">Relative (top: 20px, left: 30px)</div>
          <div class="absolute-box">Absolute (top: 50px, right: 50px)</div>
        </div>

        <!-- Z-index Demo -->
        <div class="stack-container">
          <div class="layer layer-1">Layer 1 (z-index: 1)</div>
          <div class="layer layer-2">Layer 2 (z-index: 2)</div>
          <div class="layer layer-3">Layer 3 (z-index: 3)</div>
        </div>

        <!-- Multi-column -->
        <article class="article">
          <h2>Multi-column Article</h2>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua...
          </p>
          <p>
            Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
            nisi ut aliquip ex ea commodo consequat...
          </p>
        </article>
      </main>

      <aside class="sidebar-right">
        <h3>Right Sidebar</h3>
        <div>Ads or related content</div>
      </aside>
    </div>

    <footer class="footer">
      <p>&copy; 2024 My Website</p>
    </footer>

    <!-- Fixed Element -->
    <div class="fixed-box" title="Scroll to top">↑</div>
  </body>
</html>
```

---

## 4. CSS Specificity

### Concept

**CSS Specificity** xác định quy tắc CSS nào được ưu tiên khi nhiều quy tắc áp dụng cho cùng một element.

**Thứ tự ưu tiên (cao xuống thấp):**

1. `!important` (tránh sử dụng)
2. Inline styles: `<div style="color: red">`
3. IDs: `#header`
4. Classes, attributes, pseudo-classes: `.button`, `[type="text"]`, `:hover`
5. Elements, pseudo-elements: `div`, `::before`

**Cách tính Specificity:**

- Inline: 1000
- ID: 100
- Class/Attribute/Pseudo-class: 10
- Element/Pseudo-element: 1

### Examples

```css
/* Specificity: 1 (element) */
p {
  color: black;
}

/* Specificity: 10 (class) - WINS over element */
.text {
  color: blue;
}

/* Specificity: 100 (ID) - WINS over class */
#main-text {
  color: green;
}

/* Specificity: 111 (ID + class + element) */
div#container .text {
  color: purple;
}

/* Specificity: 1000 (inline) - HIGHEST */
/* <p style="color: red">Text</p> */

/* !important - OVERRIDES EVERYTHING (avoid!) */
p {
  color: orange !important;
}

/* Examples */
/* Specificity: 1 */
div {
  color: black;
}

/* Specificity: 2 */
ul li {
  color: blue;
}

/* Specificity: 10 */
.menu {
  color: green;
}

/* Specificity: 11 (10 + 1) */
.menu li {
  color: purple;
}

/* Specificity: 20 (10 + 10) */
.menu .item {
  color: red;
}

/* Specificity: 100 */
#header {
  color: yellow;
}

/* Specificity: 101 (100 + 1) */
#header p {
  color: orange;
}

/* Specificity: 110 (100 + 10) */
#header .title {
  color: pink;
}

/* Specificity: 21 (10 + 10 + 1) */
.nav .menu li {
  color: brown;
}

/* Attribute selector: Specificity 10 */
input[type="text"] {
  border: 1px solid blue;
}

/* Pseudo-class: Specificity 10 */
a:hover {
  color: red;
}

/* Pseudo-element: Specificity 1 */
p::first-line {
  font-weight: bold;
}

/* Combined: Specificity 21 (10 + 10 + 1) */
.button.primary:hover {
  background: blue;
}
```

**Best Practices:**

```css
/* ❌ BAD: Too specific, hard to override */
body div#container .wrapper ul.nav li.item a.link {
  color: blue;
}

/* ✅ GOOD: Use single class */
.nav-link {
  color: blue;
}

/* ❌ BAD: Using !important */
.text {
  color: red !important;
}

/* ✅ GOOD: Increase specificity instead */
.container .text {
  color: red;
}

/* ✅ BEST: Use BEM methodology */
.nav {
}
.nav__item {
}
.nav__link {
}
.nav__link--active {
}
```

---

## 5. CSS Responsive

### Concept

**Responsive Design** đảm bảo website hiển thị tốt trên mọi thiết bị (desktop, tablet, mobile).

**Kỹ thuật chính:**

1. **Viewport**: `<meta name="viewport">`
2. **Media Queries**: CSS rules dựa trên screen size
3. **Flexible Units**: %, em, rem, vw, vh
4. **Flexible Images**: `max-width: 100%`
5. **Mobile-First**: Thiết kế cho mobile trước

**Common Breakpoints:**

- Mobile: < 576px
- Tablet: 576px - 768px
- Desktop: 768px - 1024px
- Large Desktop: > 1024px

### Examples

#### HTML

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <!-- IMPORTANT: Viewport meta tag -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Responsive Design</title>
    <style>
      /* Mobile-First Approach */
      /* Base styles for mobile */
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      body {
        font-family: Arial, sans-serif;
        font-size: 16px;
        line-height: 1.6;
      }

      .container {
        width: 100%;
        padding: 0 15px;
        margin: 0 auto;
      }

      /* Responsive Images */
      img {
        max-width: 100%;
        height: auto;
        display: block;
      }

      /* Grid System */
      .row {
        display: flex;
        flex-wrap: wrap;
        margin: 0 -15px;
      }

      .col {
        flex: 1;
        padding: 0 15px;
        min-width: 100%; /* Mobile: full width */
      }

      /* Navigation */
      .nav {
        background-color: #333;
        padding: 1rem;
      }

      .nav-list {
        list-style: none;
        display: flex;
        flex-direction: column; /* Mobile: vertical */
        gap: 0.5rem;
      }

      .nav-link {
        color: white;
        text-decoration: none;
        padding: 0.5rem;
        display: block;
      }

      /* Cards */
      .card-grid {
        display: grid;
        grid-template-columns: 1fr; /* Mobile: 1 column */
        gap: 1rem;
        padding: 1rem;
      }

      .card {
        border: 1px solid #ddd;
        border-radius: 8px;
        overflow: hidden;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }

      .card-content {
        padding: 1rem;
      }

      /* Flexible Units */
      h1 {
        font-size: 2rem; /* 32px if base is 16px */
      }

      h2 {
        font-size: 1.5rem; /* 24px */
      }

      .hero {
        height: 50vh; /* 50% of viewport height */
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        text-align: center;
        padding: 2rem;
      }

      /* Tablet: >= 576px */
      @media (min-width: 576px) {
        .container {
          max-width: 540px;
        }

        .col {
          min-width: 50%; /* 2 columns */
        }

        .card-grid {
          grid-template-columns: repeat(2, 1fr); /* 2 columns */
        }

        h1 {
          font-size: 2.5rem;
        }
      }

      /* Desktop: >= 768px */
      @media (min-width: 768px) {
        .container {
          max-width: 720px;
        }

        .nav-list {
          flex-direction: row; /* Horizontal navigation */
          justify-content: space-around;
        }

        .card-grid {
          grid-template-columns: repeat(3, 1fr); /* 3 columns */
        }

        .col {
          min-width: 33.333%; /* 3 columns */
        }

        h1 {
          font-size: 3rem;
        }
      }

      /* Large Desktop: >= 1024px */
      @media (min-width: 1024px) {
        .container {
          max-width: 960px;
        }

        .card-grid {
          grid-template-columns: repeat(4, 1fr); /* 4 columns */
        }

        .col {
          min-width: 25%; /* 4 columns */
        }
      }

      /* Extra Large Desktop: >= 1200px */
      @media (min-width: 1200px) {
        .container {
          max-width: 1140px;
        }
      }

      /* Print Styles */
      @media print {
        .nav,
        .sidebar {
          display: none;
        }

        body {
          font-size: 12pt;
          color: black;
        }
      }

      /* Orientation */
      @media (orientation: landscape) {
        .hero {
          height: 100vh;
        }
      }

      /* Dark Mode */
      @media (prefers-color-scheme: dark) {
        body {
          background-color: #1a1a1a;
          color: #ffffff;
        }

        .card {
          background-color: #2a2a2a;
          border-color: #444;
        }
      }

      /* Hide on mobile, show on desktop */
      .desktop-only {
        display: none;
      }

      @media (min-width: 768px) {
        .desktop-only {
          display: block;
        }
      }

      /* Show on mobile, hide on desktop */
      .mobile-only {
        display: block;
      }

      @media (min-width: 768px) {
        .mobile-only {
          display: none;
        }
      }
    </style>
  </head>
  <body>
    <nav class="nav">
      <ul class="nav-list">
        <li><a href="#" class="nav-link">Home</a></li>
        <li><a href="#" class="nav-link">About</a></li>
        <li><a href="#" class="nav-link">Services</a></li>
        <li><a href="#" class="nav-link">Contact</a></li>
      </ul>
    </nav>

    <section class="hero">
      <div>
        <h1>Responsive Design</h1>
        <p>Resize your browser to see the magic!</p>
      </div>
    </section>

    <div class="container">
      <div class="card-grid">
        <div class="card">
          <img src="https://via.placeholder.com/300x200" alt="Card 1" />
          <div class="card-content">
            <h3>Card 1</h3>
            <p>Responsive card layout</p>
          </div>
        </div>
        <div class="card">
          <img src="https://via.placeholder.com/300x200" alt="Card 2" />
          <div class="card-content">
            <h3>Card 2</h3>
            <p>Responsive card layout</p>
          </div>
        </div>
        <div class="card">
          <img src="https://via.placeholder.com/300x200" alt="Card 3" />
          <div class="card-content">
            <h3>Card 3</h3>
            <p>Responsive card layout</p>
          </div>
        </div>
        <div class="card">
          <img src="https://via.placeholder.com/300x200" alt="Card 4" />
          <div class="card-content">
            <h3>Card 4</h3>
            <p>Responsive card layout</p>
          </div>
        </div>
      </div>

      <div class="mobile-only">
        <p>This shows only on mobile</p>
      </div>

      <div class="desktop-only">
        <p>This shows only on desktop</p>
      </div>
    </div>
  </body>
</html>
```

---

## 6. Semantic HTML

### Concept

**Semantic HTML** sử dụng HTML tags có ý nghĩa rõ ràng về nội dung, giúp:

- SEO tốt hơn
- Accessibility (a11y) cho người khuyết tật
- Code dễ đọc và maintain

**Semantic Elements:**

- `<header>`: Phần đầu trang/section
- `<nav>`: Navigation links
- `<main>`: Nội dung chính
- `<article>`: Nội dung độc lập
- `<section>`: Phần của document
- `<aside>`: Nội dung phụ (sidebar)
- `<footer>`: Phần cuối trang/section
- `<figure>`, `<figcaption>`: Hình ảnh với caption
- `<time>`: Thời gian
- `<mark>`: Highlight text

### Examples

```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Trang blog về lập trình web">
  <meta name="keywords" content="HTML, CSS, JavaScript, Web Development">
  <meta name="author" content="John Doe">
  <title>Blog về Web Development</title>
</head>
<body>
  <!-- ❌ NON-SEMANTIC -->
  <!--
  <div class="header">
    <div class="nav">
      <div class="nav-item">Home</div>
    </div>
  </div>
  <div class="main">
    <div class="article">Content</div>
  </div>
  <div class="footer">Footer</div>
  -->

  <!-- ✅ SEMANTIC HTML -->
  <header>
    <h1>My Blog</h1>
    <nav>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#blog">Blog</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <!-- Article 1 -->
    <article>
      <header>
        <h2>Understanding Semantic HTML</h2>
        <p>
          Published on <time datetime="2024-01-15">January 15, 2024</time>
          by <address>John Doe</address>
        </p>
      </header>

      <section>
        <h3>Introduction</h3>
        <p>
          <mark>Semantic HTML</mark> là việc sử dụng HTML markup để
          <strong>tăng cường ý nghĩa</strong> của thông tin trong trang web.
        </p>
      </section>

      <section>
        <h3>Benefits</h3>
        <ul>
          <li><strong>SEO</strong>: Search engines hiểu nội dung tốt hơn</li>
          <li><strong>Accessibility</strong>: Screen readers đọc dễ hơn</li>
          <li><strong>Maintainability</strong>: Code dễ đọc hơn</li>
        </ul>
      </section>

      <figure>
        <img src="semantic-html.png" alt="Semantic HTML structure diagram">
        <figcaption>Hình 1: Cấu trúc Semantic HTML</figcaption>
      </figure>

      <section>
        <h3>Code Example</h3>
        <pre><code>&lt;article&gt;
  &lt;header&gt;
    &lt;h2&gt;Title&lt;/h2&gt;
  &lt;/header&gt;
  &lt;p&gt;Content&lt;/p&gt;
&lt;/article&gt;</code></pre>
      </section>

      <footer>
        <p>Tags: <a href="#html">HTML</a>, <a href="#semantic">Semantic</a></p>
      </footer>
    </article>

    <!-- Article 2 -->
    <article>
      <header>
        <h2>CSS Grid Layout</h2>
        <p>
          Published on <time datetime="2024-01-10">January 10, 2024</time>
        </p>
      </header>

      <p>CSS Grid là công cụ layout mạnh mẽ...</p>

      <aside>
        <h4>Related Articles</h4>
        <ul>
          <li><a href="#">Flexbox Guide</a></li>
          <li><a href="#">Responsive Design</a></li>
        </ul>
      </aside>
    </article>
  </main>

  <aside>
    <section>
      <h3>About Me</h3>
      <p>Tôi là một web developer với 5 năm kinh nghiệm...</p>
    </section>

    <section>
      <h3>Categories</h3>
      <nav>
        <ul>
          <li><a href="#html">HTML</a></li>
          <li><a href="#css">CSS</a></li>
          <li><a href="#javascript">JavaScript</a></li>
        </ul>
      </nav>
    </section>
  </aside>

  <footer>
    <nav>
      <ul>
        <li><a href="#privacy">Privacy Policy</a></li>
        <li><a href="#terms">Terms of Service</a></li>
      </ul>
    </nav>
    <p>&copy; <time datetime="2024">2024</time> My Blog. All rights reserved.</p>
    <address>
      Contact: <a href="mailto:contact@myblog.com">contact@myblog.com</a>
    </address>
  </footer>
</body>
</html>
```

**Semantic vs Non-Semantic:**

```html
<!-- ❌ NON-SEMANTIC -->
<div class="button">Click me</div>
<div onclick="submit()">Submit</div>
<span class="heading">Title</span>

<!-- ✅ SEMANTIC -->
<button>Click me</button>
<button type="submit">Submit</button>
<h1>Title</h1>

<!-- ❌ NON-SEMANTIC TABLE for LAYOUT -->
<table>
  <tr>
    <td>Sidebar</td>
    <td>Main Content</td>
  </tr>
</table>

<!-- ✅ SEMANTIC for DATA -->
<table>
  <caption>
    Student Grades
  </caption>
  <thead>
    <tr>
      <th>Name</th>
      <th>Grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John</td>
      <td>A</td>
    </tr>
  </tbody>
</table>

<!-- ✅ SEMANTIC for LAYOUT -->
<div class="layout">
  <aside>Sidebar</aside>
  <main>Main Content</main>
</div>
```

---

## Practice Exercises

### Bài 1: Flexbox Navigation

Tạo một navigation bar responsive sử dụng Flexbox:

- Desktop: Horizontal menu với logo bên trái, links ở giữa, button bên phải
- Mobile: Hamburger menu, vertical layout

### Bài 2: Grid Gallery

Tạo photo gallery sử dụng CSS Grid:

- Desktop: 4 columns
- Tablet: 3 columns
- Mobile: 2 columns
- Một số ảnh chiếm 2 columns (featured)

### Bài 3: Responsive Layout

Tạo blog layout responsive:

- Desktop: Sidebar trái, main content, sidebar phải
- Tablet: Main content, sidebar dưới
- Mobile: Stack tất cả content

### Bài 4: Semantic Article

Viết một blog post sử dụng semantic HTML hoàn chỉnh:

- Header với title và metadata
- Sections với headings
- Figure với images
- Footer với tags

---

## References

1. [CSS Tricks - A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
2. [CSS Tricks - A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
3. [MDN - Responsive Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design)
4. [MDN - Semantic HTML](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)
5. [CSS Specificity Calculator](https://specificity.keegan.st/)

---

**Previous Module:** [← Preparation](./01-preparation.md)  
**Next Module:** [TypeScript →](./03-typescript.md)

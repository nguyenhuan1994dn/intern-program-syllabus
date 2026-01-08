# Module 1: Preparation - Chuẩn bị

## Mục tiêu module

Module này giúp bạn làm quen với môi trường làm việc, các công cụ cần thiết và quy trình Git.

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
- JavaScript (ES6) code snippets
- Path Intellisense

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

### 3.6 Conventional Commits

#### Khái niệm

Conventional Commits là quy ước đặt tên commit message để tạo lịch sử commit rõ ràng và dễ đọc.

**Format:**

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Các types phổ biến:**

| Type       | Mô tả                                         |
| ---------- | --------------------------------------------- |
| `feat`     | Thêm feature mới                              |
| `fix`      | Sửa bug                                       |
| `docs`     | Thay đổi documentation                        |
| `style`    | Formatting, không ảnh hưởng code logic        |
| `refactor` | Refactor code không thêm feature hoặc fix bug |
| `test`     | Thêm hoặc sửa tests                           |
| `chore`    | Thay đổi build process, tools                 |

#### Ví dụ

```bash
# Feature mới
git commit -m "feat: add user authentication"
git commit -m "feat(auth): implement JWT token validation"

# Fix bug
git commit -m "fix: resolve login redirect issue"
git commit -m "fix(api): handle null response from server"

# Documentation
git commit -m "docs: update README with installation steps"

# Refactor
git commit -m "refactor: simplify user validation logic"

# Breaking change
git commit -m "feat!: change authentication API"
git commit -m "feat(api)!: rename endpoint from /users to /members"
```

### 3.7 Xử lý Merge Conflicts

#### Khái niệm

Merge conflict xảy ra khi Git không thể tự động merge các thay đổi từ các branches khác nhau.

#### Ví dụ

```bash
# Khi merge có conflict
git merge feature/login
# Auto-merging src/auth.js
# CONFLICT (content): Merge conflict in src/auth.js

# Xem files có conflict
git status

# File conflict sẽ có dạng:
<<<<<<< HEAD
// Code từ current branch
const config = { timeout: 3000 };
=======
// Code từ merging branch
const config = { timeout: 5000 };
>>>>>>> feature/login

# Sau khi resolve conflict:
git add src/auth.js
git commit -m "fix: resolve merge conflict in auth.js"
```

---

## 4. Workspace Setup

### 4.1 Node.js & npm

#### Khái niệm

Node.js là runtime environment cho JavaScript. npm (Node Package Manager) dùng để quản lý các packages/dependencies.

#### Ví dụ

```bash
# Kiểm tra version
node --version
npm --version

# Khởi tạo project mới
npm init -y

# Cài đặt dependencies
npm install react react-dom
npm install -D typescript eslint

# Cài đặt global package
npm install -g create-react-app

# Chạy scripts
npm run dev
npm run build
npm test
```

### 4.2 package.json

#### Khái niệm

File `package.json` chứa thông tin và cấu hình của project.

#### Ví dụ

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My awesome project",
  "main": "index.js",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "test": "jest",
    "lint": "eslint src/"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0"
  }
}
```

### 4.3 ESLint & Prettier

#### Khái niệm

- **ESLint**: Công cụ phát hiện lỗi và đảm bảo code quality
- **Prettier**: Công cụ format code tự động

#### Ví dụ

```bash
# Cài đặt
npm install -D eslint prettier eslint-config-prettier

# Tạo config files
npx eslint --init
```

**.eslintrc.json**:

```json
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": ["eslint:recommended", "prettier"],
  "rules": {
    "no-unused-vars": "warn",
    "no-console": "warn"
  }
}
```

**.prettierrc**:

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80
}
```

---

## Bài tập thực hành

### Bài 1: Setup môi trường

1. Cài đặt VS Code và các extensions cần thiết
2. Cài đặt Git và cấu hình user name/email
3. Cài đặt Node.js và npm
4. Verify tất cả installations

```bash
# Verify installations
code --version
git --version
node --version
npm --version
```

### Bài 2: Git Flow

1. Fork một repository trên GitHub
2. Clone repository về local
3. Tạo feature branch mới
4. Thêm một file mới và commit với conventional commit message
5. Push lên remote và tạo Pull Request

```bash
# Step by step
git clone https://github.com/your-username/repo.git
cd repo
git checkout -b feature/add-readme
echo "# My Project" > README.md
git add README.md
git commit -m "docs: add README file"
git push origin feature/add-readme
```

### Bài 3: Xử lý Git conflicts

1. Tạo 2 branches từ main
2. Sửa cùng một file ở 2 branches khác nhau
3. Merge branch thứ nhất vào main
4. Merge branch thứ hai và resolve conflicts

### Bài 4: npm & package.json

1. Khởi tạo một project mới với npm init
2. Cài đặt một vài dependencies (ví dụ: lodash, axios)
3. Tạo script trong package.json
4. Chạy script đã tạo

---

## Tài liệu tham khảo

1. [Git Documentation](https://git-scm.com/doc)
2. [GitHub Guides](https://guides.github.com/)
3. [Conventional Commits](https://www.conventionalcommits.org/)
4. [VS Code Documentation](https://code.visualstudio.com/docs)
5. [Node.js Documentation](https://nodejs.org/docs)
6. [npm Documentation](https://docs.npmjs.com/)
7. [ESLint Documentation](https://eslint.org/docs)
8. [Prettier Documentation](https://prettier.io/docs)

---

**Next Module:** [JavaScript Basics →](./02-javascript-basics.md)

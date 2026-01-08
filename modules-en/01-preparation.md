# Module 1: Preparation

## Module Objectives

This module helps you get familiar with the work environment, necessary tools, and Git workflow.

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
- JavaScript (ES6) code snippets
- Path Intellisense

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

### 3.6 Conventional Commits

#### Concept

Conventional Commits is a specification for writing standardized commit messages to create clear and readable history.

**Format:**

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Common types:**

| Type       | Description                                        |
| ---------- | -------------------------------------------------- |
| `feat`     | Add new feature                                    |
| `fix`      | Fix bug                                            |
| `docs`     | Change documentation                               |
| `style`    | Formatting, no logic change                        |
| `refactor` | Refactor code without adding feature or fixing bug |
| `test`     | Add or modify tests                                |
| `chore`    | Change build process, tools                        |

#### Examples

```bash
# New feature
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

### 3.7 Handling Merge Conflicts

#### Concept

Merge conflict occurs when Git cannot automatically merge changes from different branches.

#### Examples

```bash
# When merge has conflict
git merge feature/login
# Auto-merging src/auth.js
# CONFLICT (content): Merge conflict in src/auth.js

# View files with conflict
git status

# Conflict file will have this format:
<<<<<<< HEAD
// Code from current branch
const config = { timeout: 3000 };
=======
// Code from merging branch
const config = { timeout: 5000 };
>>>>>>> feature/login

# After resolving conflict:
git add src/auth.js
git commit -m "fix: resolve merge conflict in auth.js"
```

---

## 4. Workspace Setup

### 4.1 Node.js & npm

#### Concept

Node.js is a runtime environment for JavaScript. npm (Node Package Manager) is used to manage packages/dependencies.

#### Examples

```bash
# Check version
node --version
npm --version

# Initialize new project
npm init -y

# Install dependencies
npm install react react-dom
npm install -D typescript eslint

# Install global package
npm install -g create-react-app

# Run scripts
npm run dev
npm run build
npm test
```

### 4.2 package.json

#### Concept

The `package.json` file contains project information and configuration.

#### Examples

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

#### Concept

- **ESLint**: Tool for detecting errors and ensuring code quality
- **Prettier**: Automatic code formatting tool

#### Examples

```bash
# Installation
npm install -D eslint prettier eslint-config-prettier

# Create config files
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

## Practice Exercises

### Exercise 1: Setup environment

1. Install VS Code and necessary extensions
2. Install Git and configure user name/email
3. Install Node.js and npm
4. Verify all installations

```bash
# Verify installations
code --version
git --version
node --version
npm --version
```

### Exercise 2: Git Flow

1. Fork a repository on GitHub
2. Clone repository to local
3. Create a new feature branch
4. Add a new file and commit with conventional commit message
5. Push to remote and create Pull Request

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

### Exercise 3: Handling Git conflicts

1. Create 2 branches from main
2. Modify the same file in 2 different branches
3. Merge the first branch to main
4. Merge the second branch and resolve conflicts

### Exercise 4: npm & package.json

1. Initialize a new project with npm init
2. Install some dependencies (e.g., lodash, axios)
3. Create scripts in package.json
4. Run the created scripts

---

## References

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

# Exercise 8: GitHub Actions - Basic CI/CD

## Overview
Learn to automate your development workflow using GitHub Actions. Set up continuous integration and continuous deployment (CI/CD) to automatically test and deploy your code.

## Prerequisites
- Completed Exercise 7: Code Reviews
- GitHub repository
- Basic understanding of YAML
- Knowledge of Git and Pull Requests

## Learning Objectives
- Understand what CI/CD is and why it's important
- Create GitHub Actions workflows
- Automate testing and linting
- Set up deployment pipelines
- Use GitHub Actions marketplace
- Debug workflow failures

## What is CI/CD?

**Continuous Integration (CI)**: Automatically test code changes when they're pushed
**Continuous Deployment (CD)**: Automatically deploy code when tests pass

**Benefits:**
- Catch bugs early
- Ensure code quality
- Automate repetitive tasks
- Deploy confidently
- Faster development cycle

## Instructions

### Step 1: Create a Repository with Code

```bash
# Create new repo on GitHub: github-actions-demo
git clone https://github.com/YOUR-USERNAME/github-actions-demo.git
cd github-actions-demo
```

Create a simple Node.js project:

```bash
# Initialize npm project
npm init -y

# Create source file
mkdir src
cat > src/math.js << 'EOF'
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

function multiply(a, b) {
    return a * b;
}

function divide(a, b) {
    if (b === 0) {
        throw new Error('Division by zero');
    }
    return a / b;
}

module.exports = { add, subtract, multiply, divide };
EOF
```

### Step 2: Add Tests

Install Jest:
```bash
npm install --save-dev jest
```

Update package.json to add test script:
```json
{
  "name": "github-actions-demo",
  "version": "1.0.0",
  "description": "Demo for GitHub Actions",
  "main": "src/math.js",
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

Create test file:
```bash
mkdir tests
cat > tests/math.test.js << 'EOF'
const { add, subtract, multiply, divide } = require('../src/math');

describe('Math Operations', () => {
    test('adds two numbers correctly', () => {
        expect(add(2, 3)).toBe(5);
        expect(add(-1, 1)).toBe(0);
    });

    test('subtracts two numbers correctly', () => {
        expect(subtract(5, 3)).toBe(2);
        expect(subtract(0, 5)).toBe(-5);
    });

    test('multiplies two numbers correctly', () => {
        expect(multiply(4, 5)).toBe(20);
        expect(multiply(-2, 3)).toBe(-6);
    });

    test('divides two numbers correctly', () => {
        expect(divide(10, 2)).toBe(5);
        expect(divide(7, 2)).toBe(3.5);
    });

    test('throws error when dividing by zero', () => {
        expect(() => divide(10, 0)).toThrow('Division by zero');
    });
});
EOF
```

Test locally:
```bash
npm test
```

Commit and push:
```bash
git add .
git commit -m "Add math module with tests"
git push origin main
```

### Step 3: Create Your First Workflow

Create workflow directory:
```bash
mkdir -p .github/workflows
```

Create workflow file `.github/workflows/test.yml`:
```yaml
name: Run Tests

# When to run this workflow
on:
  # Run on push to main branch
  push:
    branches: [ main ]
  # Run on pull requests to main
  pull_request:
    branches: [ main ]

# Jobs to run
jobs:
  test:
    # Run on Ubuntu
    runs-on: ubuntu-latest
    
    steps:
    # Step 1: Checkout code
    - name: Checkout code
      uses: actions/checkout@v3
    
    # Step 2: Setup Node.js
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    # Step 3: Install dependencies
    - name: Install dependencies
      run: npm install
    
    # Step 4: Run tests
    - name: Run tests
      run: npm test
```

Commit and push:
```bash
git add .github/workflows/test.yml
git commit -m "Add GitHub Actions workflow for testing"
git push origin main
```

### Step 4: View Workflow Results

1. Go to your repository on GitHub
2. Click the "Actions" tab
3. You should see your workflow running
4. Click on the workflow run to see details
5. Expand each step to see output

### Step 5: Create a Workflow with Multiple Jobs

Create `.github/workflows/ci.yml`:
```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  lint:
    name: Lint Code
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm install
    
    - name: Install ESLint
      run: npm install --save-dev eslint
    
    - name: Run ESLint
      run: npx eslint src/ --ext .js || true
      # || true allows the workflow to continue even if linting fails

  test:
    name: Run Tests
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Generate coverage report
      run: npm test -- --coverage
      
  build:
    name: Build Project
    runs-on: ubuntu-latest
    needs: [lint, test]  # Only run if lint and test pass
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm install
    
    - name: Build (if applicable)
      run: echo "Build successful!"
```

### Step 6: Test with a Pull Request

Create a feature branch:
```bash
git checkout -b feature/add-power-function
```

Add a new function with a bug:
```javascript
// Add to src/math.js
function power(base, exponent) {
    return Math.pow(base, exponent);
}

module.exports = { add, subtract, multiply, divide, power };
```

Add test:
```javascript
// Add to tests/math.test.js
test('calculates power correctly', () => {
    expect(power(2, 3)).toBe(8);
    expect(power(5, 2)).toBe(25);
});
```

Commit and push:
```bash
git add .
git commit -m "Add power function"
git push -u origin feature/add-power-function
```

Create a PR and watch the workflow run!

### Step 7: Add Status Badges

Add a badge to your README:

```markdown
# GitHub Actions Demo

![CI Pipeline](https://github.com/YOUR-USERNAME/github-actions-demo/workflows/CI%20Pipeline/badge.svg)

A demo project showcasing GitHub Actions for CI/CD.

## Status

- ✅ Automated testing
- ✅ Linting
- ✅ Build verification
```

## Common Workflow Patterns

### Pattern 1: Test on Multiple Node Versions

```yaml
name: Test Multiple Versions

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm install
    - run: npm test
```

### Pattern 2: Test on Multiple Operating Systems

```yaml
name: Cross-Platform Tests

on: [push, pull_request]

jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    runs-on: ${{ matrix.os }}
    
    steps:
    - uses: actions/checkout@v3
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    - run: npm install
    - run: npm test
```

### Pattern 3: Deploy on Success

```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to Production
      run: |
        echo "Deploying to production..."
        # Your deployment commands here
```

### Pattern 4: Scheduled Workflows

```yaml
name: Nightly Build

on:
  schedule:
    # Run at 2 AM every day
    - cron: '0 2 * * *'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - run: npm install
    - run: npm test
```

## Useful GitHub Actions

### From the Marketplace

```yaml
# Code coverage
- name: Upload coverage to Codecov
  uses: codecov/codecov-action@v3
  
# Slack notifications
- name: Slack Notification
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    
# Docker build and push
- name: Build and push Docker image
  uses: docker/build-push-action@v4
  
# Deploy to GitHub Pages
- name: Deploy to GitHub Pages
  uses: peaceiris/actions-gh-pages@v3
```

## Environment Variables and Secrets

### Using Environment Variables

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      NODE_ENV: production
      API_URL: https://api.example.com
    
    steps:
    - name: Use environment variable
      run: echo "API URL is $API_URL"
```

### Using Secrets

1. Go to repository Settings → Secrets → Actions
2. Click "New repository secret"
3. Add secret (e.g., `API_KEY`)

Use in workflow:
```yaml
steps:
- name: Deploy with API key
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: |
    echo "Using API key: ${API_KEY:0:5}..."
```

## Practice Exercise

Create a complete CI/CD pipeline:

1. **Create Project**
   - Simple web application
   - Unit tests
   - Integration tests

2. **Setup Workflows**
   - Lint on every push
   - Test on PRs
   - Deploy on merge to main

3. **Add Features**
   - Code coverage reporting
   - Automated versioning
   - Release creation

4. **Configure Protection**
   - Require CI to pass before merge
   - Add CODEOWNERS file
   - Enable branch protection

## Challenge Tasks

### 1. Create a Monorepo Workflow
Test only changed packages in a monorepo.

### 2. Add Docker Build
Build and push Docker images on release.

### 3. Implement GitFlow
Workflows for develop, main, and feature branches.

### 4. Add Performance Testing
Run performance benchmarks and comment results on PRs.

### 5. Create Custom Action
Build your own reusable GitHub Action.

## Debugging Workflows

### Enable Debug Logging

```yaml
- name: Debug step
  run: |
    echo "::debug::This is a debug message"
    echo "::warning::This is a warning"
    echo "::error::This is an error"
```

### View Runner Logs
1. Go to Actions tab
2. Click on failed workflow
3. Click on failed job
4. Expand failed step
5. Look for error messages

### Common Issues

**Problem**: Dependencies not cached
**Solution**:
```yaml
- uses: actions/setup-node@v3
  with:
    node-version: '18'
    cache: 'npm'
```

**Problem**: Tests fail in CI but pass locally
**Solution**: Check environment variables and test isolation

**Problem**: Workflow doesn't trigger
**Solution**: Check branch names and trigger conditions

## Best Practices

### 1. Fail Fast
```yaml
strategy:
  fail-fast: true  # Stop all jobs if one fails
```

### 2. Use Caching
```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

### 3. Separate Concerns
```
.github/workflows/
  ├── test.yml          # Testing only
  ├── lint.yml          # Linting only
  ├── deploy.yml        # Deployment only
  └── release.yml       # Release automation
```

### 4. Use Conditional Execution
```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: npm run deploy
```

### 5. Document Your Workflows
```yaml
# This workflow runs tests on every push and PR
# It tests on multiple Node versions to ensure compatibility
name: Test Suite
```

## Expected Output
- Automated testing on every push
- PR status checks
- Successful CI/CD pipeline
- Understanding of GitHub Actions

## Next Steps
Continue to [Exercise 9: C# Console App Hello World](09-csharp-console-hello-world.md) to start learning C# development!

## Resources
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Actions Marketplace](https://github.com/marketplace?type=actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Awesome Actions](https://github.com/sdras/awesome-actions)

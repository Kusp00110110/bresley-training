# Exercise 7: GitHub Workflow - Code Reviews and Merging

## Overview
Master the art of code reviews and learn different merging strategies. Code review is one of the most important skills in collaborative software development!

## Prerequisites
- Completed Exercise 6: GitHub Pull Requests
- GitHub account
- Understanding of PRs and Git branching
- Ability to read and understand code

## Learning Objectives
- Conduct effective code reviews
- Provide constructive feedback
- Understand different merge strategies
- Learn review etiquette and best practices
- Use GitHub's review tools effectively

## What is Code Review?

Code review is the process of examining code written by others (or yourself) to:
- **Catch Bugs**: Find issues before they reach production
- **Improve Quality**: Suggest better approaches
- **Share Knowledge**: Learn from each other
- **Maintain Standards**: Ensure code follows team conventions
- **Build Better Software**: Two pairs of eyes are better than one

## Instructions

### Step 1: Set Up Review Practice Repository

```bash
# Create new repo on GitHub: code-review-practice
git clone https://github.com/YOUR-USERNAME/code-review-practice.git
cd code-review-practice

# Create initial structure
mkdir -p src tests docs
echo "# Code Review Practice" > README.md
git add .
git commit -m "Initial project structure"
git push origin main
```

### Step 2: Create a PR to Review

Create a branch with code that needs review:

```bash
git checkout -b feature/add-calculator
```

Create `src/calculator.js`:
```javascript
// Simple calculator with intentional issues for review practice
function add(a, b) {
    return a + b;
}

function subtract(x, y) {
    return x - y;
}

function multiply(num1, num2) {
    return num1 * num2;
}

function divide(a, b) {
    return a / b;  // Issue: No check for division by zero
}

function calculate(operation, a, b) {
    if (operation == "add") {  // Issue: Using == instead of ===
        return add(a, b);
    } else if (operation == "subtract") {
        return subtract(a, b);
    } else if (operation == "multiply") {
        return multiply(a, b);
    } else if (operation == "divide") {
        return divide(a, b);
    }
    // Issue: No return statement for invalid operations
}

module.exports = { add, subtract, multiply, divide, calculate };
```

Create `tests/calculator.test.js`:
```javascript
const calc = require('../src/calculator');

// Test add function
console.log('Testing add...');
console.assert(calc.add(2, 3) === 5, 'Add test failed');
console.log('Add tests passed!');

// Test subtract
console.log('Testing subtract...');
console.assert(calc.subtract(5, 3) === 2, 'Subtract test failed');
console.log('Subtract tests passed!');

// Test multiply
console.log('Testing multiply...');
console.assert(calc.multiply(4, 5) === 20, 'Multiply test failed');
console.log('Multiply tests passed!');

// Test divide (missing edge cases!)
console.log('Testing divide...');
console.assert(calc.divide(10, 2) === 5, 'Divide test failed');
console.log('Divide tests passed!');

console.log('All tests passed!');
```

Commit and push:
```bash
git add src/calculator.js tests/calculator.test.js
git commit -m "Add calculator module with basic operations"
git push -u origin feature/add-calculator
```

### Step 3: Create the Pull Request

1. Go to GitHub and create a PR
2. Title: "Add calculator module"
3. Description:
```markdown
## Description
Implements a basic calculator with add, subtract, multiply, and divide operations.

## Changes
- Added calculator.js with four basic operations
- Added test file with basic test cases
- Exported functions for use in other modules

## Testing
- [x] Ran manual tests
- [x] All operations work correctly

## Questions for Reviewers
- Is the error handling sufficient?
- Should we add more test cases?
```

### Step 4: Conduct a Code Review

Now review your own PR to practice! Click "Files changed" tab.

**Add review comments:**

1. **On the divide function**, click the + icon and comment:
```
💡 Suggestion: Add check for division by zero

What happens if b is 0? This will return Infinity, which might not be the desired behavior.

Suggested fix:
```javascript
function divide(a, b) {
    if (b === 0) {
        throw new Error('Cannot divide by zero');
    }
    return a / b;
}
```
```

2. **On the equality check**, comment:
```
🐛 Bug: Use strict equality

Using `==` can lead to unexpected type coercion. Use `===` instead.

Change `if (operation == "add")` to `if (operation === "add")`
```

3. **On the calculate function**, comment:
```
⚠️ Issue: Missing return for invalid operations

If an invalid operation is passed, the function returns `undefined`. 
Consider throwing an error or returning a default value.

Example:
```javascript
throw new Error(`Invalid operation: ${operation}`);
```
```

4. **On the test file**, comment:
```
🧪 Missing test cases

Consider adding tests for:
- Division by zero
- Invalid operations in calculate()
- Negative numbers
- Decimal numbers
- Large numbers

This will make the code more robust.
```

### Step 5: Submit Your Review

1. Click "Review changes" button (top right)
2. Write overall feedback:
```
Good start on the calculator! Found a few issues:
- Division by zero not handled
- Using == instead of ===
- Missing error handling for invalid operations
- Tests could be more comprehensive

Overall structure is clean. Let me know if you have questions!
```

3. Select "Comment" or "Request changes"
4. Click "Submit review"

### Step 6: Address Review Feedback

Make changes based on the review:

```bash
git checkout feature/add-calculator
```

Update `src/calculator.js`:
```javascript
// Calculator with improved error handling
function add(a, b) {
    return a + b;
}

function subtract(x, y) {
    return x - y;
}

function multiply(num1, num2) {
    return num1 * num2;
}

function divide(a, b) {
    if (b === 0) {
        throw new Error('Cannot divide by zero');
    }
    return a / b;
}

function calculate(operation, a, b) {
    const validOperations = ['add', 'subtract', 'multiply', 'divide'];
    
    if (!validOperations.includes(operation)) {
        throw new Error(`Invalid operation: ${operation}. Valid operations are: ${validOperations.join(', ')}`);
    }
    
    if (operation === "add") {
        return add(a, b);
    } else if (operation === "subtract") {
        return subtract(a, b);
    } else if (operation === "multiply") {
        return multiply(a, b);
    } else if (operation === "divide") {
        return divide(a, b);
    }
}

module.exports = { add, subtract, multiply, divide, calculate };
```

Update `tests/calculator.test.js`:
```javascript
const calc = require('../src/calculator');

function runTests() {
    let passed = 0;
    let failed = 0;
    
    function test(description, condition) {
        if (condition) {
            console.log(`✓ ${description}`);
            passed++;
        } else {
            console.log(`✗ ${description}`);
            failed++;
        }
    }
    
    // Basic operations
    test('Add: 2 + 3 = 5', calc.add(2, 3) === 5);
    test('Subtract: 5 - 3 = 2', calc.subtract(5, 3) === 2);
    test('Multiply: 4 * 5 = 20', calc.multiply(4, 5) === 20);
    test('Divide: 10 / 2 = 5', calc.divide(10, 2) === 5);
    
    // Edge cases
    test('Add negative numbers', calc.add(-5, -3) === -8);
    test('Divide with decimals', calc.divide(7, 2) === 3.5);
    
    // Error handling
    test('Divide by zero throws error', (() => {
        try {
            calc.divide(10, 0);
            return false;
        } catch (e) {
            return e.message === 'Cannot divide by zero';
        }
    })());
    
    test('Invalid operation throws error', (() => {
        try {
            calc.calculate('power', 2, 3);
            return false;
        } catch (e) {
            return e.message.includes('Invalid operation');
        }
    })());
    
    test('Calculate add works', calc.calculate('add', 5, 3) === 8);
    test('Calculate divide works', calc.calculate('divide', 20, 4) === 5);
    
    console.log(`\n${passed} passed, ${failed} failed`);
    return failed === 0;
}

runTests();
```

Commit the fixes:
```bash
git add src/calculator.js tests/calculator.test.js
git commit -m "Address code review feedback

- Add division by zero check
- Use strict equality (===)
- Add error handling for invalid operations
- Expand test coverage with edge cases"
git push
```

### Step 7: Re-review and Approve

1. Go back to the PR on GitHub
2. Check the new commits
3. Review the changes
4. If satisfied, click "Review changes"
5. Add comment: "LGTM! 🚀 (Looks Good To Me)"
6. Select "Approve"
7. Click "Submit review"

### Step 8: Merge with Different Strategies

GitHub offers three merge strategies:

**1. Create a Merge Commit** (default)
```
Preserves all commits and creates a merge commit
Best for: Feature branches, maintaining history
```

**2. Squash and Merge**
```
Combines all commits into one
Best for: Cleaning up messy commit history
```

**3. Rebase and Merge**
```
Replays commits on top of base branch
Best for: Linear history, no merge commits
```

Try squash and merge:
1. Click "Squash and merge"
2. Edit the commit message if needed
3. Click "Confirm squash and merge"

## Code Review Best Practices

### For Reviewers

#### What to Look For

**🐛 Bugs and Logic Errors**
```
- Off-by-one errors
- Null pointer exceptions
- Infinite loops
- Race conditions
```

**📚 Code Quality**
```
- Naming conventions
- Code duplication
- Function complexity
- Proper error handling
```

**🧪 Testing**
```
- Test coverage
- Edge cases
- Error scenarios
- Integration points
```

**📖 Documentation**
```
- Clear comments
- Updated README
- API documentation
- Inline explanations
```

**🔒 Security**
```
- Input validation
- SQL injection risks
- XSS vulnerabilities
- Exposed secrets
```

**⚡ Performance**
```
- Inefficient algorithms
- Memory leaks
- Unnecessary computations
- Database query optimization
```

#### How to Give Feedback

✅ **Good Feedback:**
```
"Consider using Array.map() instead of forEach() here for better 
readability and functional approach. Example:

const results = items.map(item => item.value);
```

❌ **Bad Feedback:**
```
"This is wrong."
```

✅ **Good Feedback:**
```
"Great job handling the edge case for empty arrays! One suggestion: 
we could extract this validation into a separate function to make 
it reusable."
```

❌ **Bad Feedback:**
```
"You should have done it this way..."
```

### Review Comment Types

Use these prefixes for clarity:

```
🐛 Bug: Critical issue that must be fixed
⚠️ Issue: Problem that should be addressed
💡 Suggestion: Optional improvement
❓ Question: Asking for clarification
💬 Note: General comment
✨ Praise: Positive feedback
🧪 Testing: Test-related comment
📖 Docs: Documentation comment
♻️ Refactor: Code structure improvement
```

### The "Yes, and..." Technique

Instead of:
```
"This approach is wrong. Use this instead..."
```

Try:
```
"This works, and we could also consider [alternative] which 
might provide [benefit]. What do you think?"
```

### For Authors (Receiving Reviews)

**✅ Do:**
- Thank reviewers for their time
- Ask questions if feedback is unclear
- Consider all feedback seriously
- Explain your reasoning if you disagree
- Make requested changes promptly

**❌ Don't:**
- Take feedback personally
- Ignore suggestions without explanation
- Get defensive
- Rush to merge without addressing issues
- Dismiss feedback as "nitpicking"

## Practice Exercise: Review Scenarios

### Scenario 1: Security Issue
```javascript
// Review this code
app.get('/user/:id', (req, res) => {
    const query = `SELECT * FROM users WHERE id = ${req.params.id}`;
    db.query(query, (err, results) => {
        res.json(results);
    });
});
```

**Your review comments:**
```
🐛 Security Bug: SQL Injection Vulnerability

This code is vulnerable to SQL injection. Use parameterized queries:

```javascript
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [req.params.id], (err, results) => {
    res.json(results);
});
```
```

### Scenario 2: Code Quality
```javascript
function processData(d) {
    let r = [];
    for(let i=0;i<d.length;i++){
        if(d[i].active==true&&d[i].value>0){
            r.push(d[i]);
        }
    }
    return r;
}
```

**Your review comments:**
```
♻️ Refactor: Improve readability and naming

The logic is correct but could be more readable:

```javascript
function filterActivePositiveItems(data) {
    return data.filter(item => 
        item.active === true && item.value > 0
    );
}
```

Benefits:
- Descriptive function name
- Clearer variable names
- More functional approach
- Easier to test
```

### Scenario 3: Missing Tests
```javascript
// New feature: password validation
function isPasswordStrong(password) {
    return password.length >= 8 && 
           /[A-Z]/.test(password) && 
           /[0-9]/.test(password);
}
```

**Your review comments:**
```
🧪 Missing Tests

This function needs comprehensive tests:

```javascript
// Suggested tests
test('accepts strong password', () => {
    expect(isPasswordStrong('Password123')).toBe(true);
});

test('rejects short password', () => {
    expect(isPasswordStrong('Pass1')).toBe(false);
});

test('rejects password without uppercase', () => {
    expect(isPasswordStrong('password123')).toBe(false);
});

test('rejects password without number', () => {
    expect(isPasswordStrong('Password')).toBe(false);
});
```

Also consider: special characters requirement?


## Advanced Review Techniques

### 1. Suggesting Code Changes
GitHub lets you suggest changes directly:

````markdown
```suggestion
const result = items.filter(item => item.active);
```
````

### 2. Batch Comments
Hold comments and submit them all at once instead of one-by-one.

### 3. Using Review Templates
Create `.github/PULL_REQUEST_TEMPLATE/code_review.md`:

```markdown
## Code Review Checklist

- [ ] Code follows project style guidelines
- [ ] Tests cover new functionality
- [ ] Documentation updated
- [ ] No security vulnerabilities
- [ ] Performance considerations addressed
- [ ] Error handling present
- [ ] Naming is clear and consistent
```

### 4. Automated Reviews
Set up tools like:
- **CodeClimate**: Code quality analysis
- **SonarQube**: Bug and vulnerability detection
- **ESLint/Prettier**: Code formatting
- **GitHub Actions**: Automated tests

## Expected Output
- Ability to conduct thorough code reviews
- Understanding of different merge strategies
- Skills to give constructive feedback
- Knowledge of what to look for in reviews

## Challenge Tasks

### 1. Review a Real Project
Find an open-source project and review an open PR (without submitting unless asked).

### 2. Pair Review
Work with a friend:
- They create a PR
- You review it
- Discuss feedback together

### 3. Create Review Guidelines
Write code review guidelines for a team including:
- What to review
- How to give feedback
- Response time expectations

### 4. Practice All Merge Types
Create three PRs and merge each with a different strategy:
- Regular merge
- Squash merge
- Rebase merge

Compare the resulting git history.

## Common Review Mistakes

❌ **Bikeshedding**: Spending too much time on trivial issues
✅ Focus on important issues first

❌ **Being Too Harsh**: "This code is terrible"
✅ Be constructive: "Consider this alternative approach..."

❌ **Approving Without Reading**: Rubber-stamping
✅ Actually review the code

❌ **Nitpicking Everything**: 50 minor comments
✅ Focus on significant issues, suggest style guides for the rest

## Troubleshooting

**Problem**: Too many review comments overwhelming
**Solution**: Prioritize issues, group similar feedback

**Problem**: Author is defensive about feedback
**Solution**: Focus on code, not person. Use questions instead of statements

**Problem**: Review taking too long
**Solution**: Set time limits, focus on critical issues first

## Next Steps
Continue to [Exercise 8: GitHub Actions - Basic CI/CD](08-github-actions.md) to automate your workflow!

## Resources
- [Google's Code Review Guide](https://google.github.io/eng-practices/review/)
- [Conventional Comments](https://conventionalcomments.org/)
- [How to Review Code Effectively](https://github.com/features/code-review/)
- [The Art of Code Review](https://www.alexandra-hill.com/2018/06/25/the-art-of-giving-and-receiving-code-reviews/)

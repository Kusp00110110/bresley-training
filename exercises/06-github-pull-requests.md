# Exercise 6: GitHub Workflow - Creating Pull Requests

## Overview
Learn how to use GitHub Pull Requests (PRs) for code review and collaboration. PRs are the heart of collaborative development on GitHub!

## Prerequisites
- Completed Exercise 5: Git Branching
- GitHub account
- Understanding of branches and merging
- Basic Git command line skills

## Learning Objectives
- Understand what Pull Requests are and why they're important
- Create and manage Pull Requests
- Write effective PR descriptions
- Review code changes
- Use PR templates
- Understand the PR workflow

## What is a Pull Request?

A Pull Request (PR) is a way to propose changes to a codebase. It's called a "pull request" because you're asking the project maintainer to "pull" your changes.

**Key Benefits:**
- **Code Review**: Others can review your code before merging
- **Discussion**: Team can discuss changes and suggest improvements
- **Testing**: Automated tests can run before merging
- **Documentation**: PRs document why changes were made
- **Quality Control**: Prevents bad code from reaching production

## Instructions

### Step 1: Create a New Repository

```bash
# Create new repo on GitHub: github-pr-practice
# Include README when creating
git clone https://github.com/YOUR-USERNAME/github-pr-practice.git
cd github-pr-practice
```

### Step 2: Create a Feature Branch

```bash
git checkout -b feature/add-welcome-page
```

Create a new HTML file:
```bash
cat > welcome.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
</head>
<body>
    <h1>Welcome to Our Project!</h1>
    <p>This page was added via a Pull Request.</p>
</body>
</html>
EOF
```

Commit and push:
```bash
git add welcome.html
git commit -m "Add welcome page with greeting"
git push -u origin feature/add-welcome-page
```

### Step 3: Create Your First Pull Request

1. Go to your repository on GitHub
2. You'll see a yellow banner: "Compare & pull request" - click it
3. You'll see the "Open a pull request" page

**Fill in the PR details:**

**Title:**
```
Add welcome page for new users
```

**Description:**
```markdown
## Description
This PR adds a welcome page to greet new users when they visit the site.

## Changes Made
- Created `welcome.html` with welcome message
- Added basic HTML structure
- Included responsive meta tag

## Testing
- [ ] Opened `welcome.html` in browser
- [ ] Verified text displays correctly
- [ ] Checked HTML validation

## Screenshots
(If applicable, add screenshots here)

## Related Issues
Closes #1 (if you have an issue)
```

4. Click "Create pull request"

### Step 4: Explore the Pull Request Interface

On your PR page, you'll see several tabs:

**Conversation**: Discussion and comments
**Commits**: All commits in this PR
**Checks**: Automated tests (if configured)
**Files changed**: Visual diff of changes

### Step 5: Make Changes Based on Review

Let's say you need to make improvements. Add CSS:

```bash
# Make sure you're on your feature branch
git checkout feature/add-welcome-page

# Create CSS file
cat > styles.css << 'EOF'
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 50px auto;
    padding: 20px;
    text-align: center;
}

h1 {
    color: #2c3e50;
}
EOF
```

Update welcome.html to include CSS:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Welcome to Our Project!</h1>
    <p>This page was added via a Pull Request.</p>
</body>
</html>
```

Commit and push:
```bash
git add welcome.html styles.css
git commit -m "Add CSS styling to welcome page"
git push
```

The PR automatically updates with your new commit!

### Step 6: Review the Changes

Go back to your PR on GitHub:
1. Click "Files changed" tab
2. You'll see a diff of all changes
3. You can comment on specific lines by clicking the + icon

### Step 7: Merge the Pull Request

Once you're satisfied:
1. Go to the "Conversation" tab
2. Click "Merge pull request"
3. Click "Confirm merge"
4. Optionally, click "Delete branch" to clean up

### Step 8: Update Your Local Repository

```bash
# Switch to main branch
git checkout main

# Pull the merged changes
git pull origin main

# Delete local feature branch
git branch -d feature/add-welcome-page
```

## Creating a PR Template

Create `.github/pull_request_template.md` in your repository:

```bash
mkdir -p .github
cat > .github/pull_request_template.md << 'EOF'
## Description
<!-- Describe your changes in detail -->

## Type of Change
<!-- Mark with an 'x' -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## How Has This Been Tested?
<!-- Describe the tests you ran -->
- [ ] Test A
- [ ] Test B

## Checklist
- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes

## Screenshots (if appropriate)

## Additional Notes
<!-- Any additional information -->
EOF
```

Commit this template:
```bash
git add .github/pull_request_template.md
git commit -m "Add PR template"
git push origin main
```

Now every new PR will use this template!

## Practice Exercise: Complete PR Workflow

Create a complete feature using PR best practices:

1. **Create an Issue** (on GitHub)
   - Title: "Add contact form"
   - Description: User story and acceptance criteria

2. **Create Feature Branch**
   ```bash
   git checkout -b feature/add-contact-form
   ```

3. **Implement Feature**
   - Create contact.html with a form
   - Add CSS styling
   - Make multiple commits with clear messages

4. **Push and Create PR**
   ```bash
   git push -u origin feature/add-contact-form
   ```
   - Create PR with detailed description
   - Link to the issue: "Closes #[issue-number]"

5. **Request Review**
   - Add yourself as reviewer (or a friend)
   - Review your own code critically

6. **Make Improvements**
   - Add form validation with JavaScript
   - Push additional commits

7. **Merge PR**
   - Merge when ready
   - Delete branch

8. **Verify Issue Closed**
   - Check that issue was automatically closed

## Advanced PR Techniques

### 1. Draft Pull Requests
For work-in-progress:
```
Click "Create draft pull request" instead of "Create pull request"
```

### 2. Requesting Reviews
```
On PR page → "Reviewers" → Select team members
```

### 3. Adding Labels
```
On PR page → "Labels" → Select relevant labels
- bug
- enhancement
- documentation
```

### 4. Linking Issues
In PR description:
```markdown
Fixes #123
Closes #456
Resolves #789
```

### 5. PR Comments with Suggestions
Reviewers can suggest code changes that you can commit directly from GitHub!

### 6. Comparing Branches
```
https://github.com/username/repo/compare/branch1...branch2
```

## PR Best Practices

### Writing Good PR Descriptions

✅ **Good PR Description:**
```markdown
## Description
Adds user authentication using JWT tokens

## Changes
- Implemented login endpoint
- Added JWT token generation
- Created middleware for auth verification
- Updated user model with password hashing

## Testing
Tested login flow with:
- Valid credentials
- Invalid credentials
- Missing fields
- Expired tokens

## Breaking Changes
None

## Related Issues
Closes #45
```

❌ **Bad PR Description:**
```
updates
```

### PR Title Guidelines

✅ Good:
- "Add user authentication with JWT"
- "Fix null pointer exception in payment processing"
- "Update documentation for API endpoints"

❌ Bad:
- "Update"
- "Fix stuff"
- "Changes"

### When to Create a PR

✅ Do create PRs for:
- New features
- Bug fixes
- Documentation updates
- Refactoring
- Performance improvements

❌ Don't create PRs for:
- Merge conflicts (fix them first)
- Half-finished work (use draft PRs)
- Personal experiments (use your fork)

### PR Size Guidelines

**Small PR** (< 200 lines changed):
- ✅ Easy to review
- ✅ Quick to merge
- ✅ Fewer bugs

**Large PR** (> 500 lines changed):
- ❌ Hard to review
- ❌ Slow to merge
- ❌ More likely to have bugs

**Tip**: Break large features into smaller PRs!

## Common PR Workflows

### Workflow 1: Feature Development
```
1. Create issue
2. Create branch from main
3. Implement feature
4. Create PR
5. Code review
6. Address feedback
7. Merge PR
8. Delete branch
```

### Workflow 2: Bug Fix
```
1. Report/confirm bug
2. Create hotfix branch
3. Fix bug
4. Add test
5. Create PR
6. Quick review
7. Merge to main
8. Tag release
```

### Workflow 3: Documentation
```
1. Identify documentation need
2. Create docs branch
3. Write documentation
4. Create PR
5. Review for accuracy
6. Merge
```

## Challenge Tasks

### 1. Create a Multi-Commit PR
Create a PR with at least 5 meaningful commits, each with a clear purpose.

### 2. Practice Code Review
- Create a PR with intentional issues
- Review it and leave comments
- Fix the issues based on your own review

### 3. Use GitHub CLI
Install GitHub CLI and create PRs from command line:
```bash
gh pr create --title "Add feature" --body "Description"
gh pr list
gh pr view 1
gh pr merge 1
```

### 4. Set Up Branch Protection
- Require PR reviews before merging
- Require status checks to pass
- Require up-to-date branches

### 5. Create PR with Multiple Files
Add a complete feature with:
- HTML file
- CSS file
- JavaScript file
- README update
All in one PR

## Expected Output
- Successfully created and merged Pull Requests
- Understanding of PR workflow
- Ability to review code changes
- Knowledge of PR best practices

## Troubleshooting

**Problem**: PR shows too many commits
**Solution**: You might be comparing wrong branches or need to rebase

**Problem**: Can't create PR
**Solution**: 
- Ensure you pushed your branch
- Check you're not creating PR from main to main

**Problem**: PR shows conflicts
**Solution**:
```bash
git checkout main
git pull
git checkout your-branch
git merge main
# Resolve conflicts
git push
```

## Next Steps
Continue to [Exercise 7: GitHub Workflow - Code Reviews](07-github-code-reviews.md) to learn advanced review techniques!

## Resources
- [GitHub PR Documentation](https://docs.github.com/en/pull-requests)
- [How to Write the Perfect PR](https://github.blog/2015-01-21-how-to-write-the-perfect-pull-request/)
- [PR Review Best Practices](https://google.github.io/eng-practices/review/)
- [Conventional Commits](https://www.conventionalcommits.org/)

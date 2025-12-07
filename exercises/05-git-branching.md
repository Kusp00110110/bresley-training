# Exercise 5: Git/VCS - Branching and Merging

## Overview
Learn how to use Git branches to work on features independently, and merge them back together. Branching is one of Git's most powerful features!

## Prerequisites
- Completed Exercise 4: Git/VCS Basics
- Understanding of basic Git commands (add, commit, push)
- An existing Git repository

## Learning Objectives
- Understand what branches are and why they're useful
- Create and switch between branches
- Merge branches together
- Resolve merge conflicts
- Understand different branching strategies

## What are Branches?

A branch is an independent line of development. Think of it as a parallel universe where you can:
- Work on a new feature without affecting the main code
- Experiment safely
- Collaborate with others on different features simultaneously
- Keep the main branch stable and deployable

**Real-world analogy**: Imagine writing a book. The `main` branch is your published version. When you want to add a new chapter, you create a branch, write the chapter, review it, then merge it back to the main book.

## Instructions

### Step 1: Create a Practice Repository

```bash
# Create new repo on GitHub: git-branching-practice
git clone https://github.com/YOUR-USERNAME/git-branching-practice.git
cd git-branching-practice
```

Create initial files:
```bash
echo "# Git Branching Practice" > README.md
echo "Version 1.0" > version.txt
git add .
git commit -m "Initial commit"
git push origin main
```

### Step 2: Understanding Branch Basics

View current branches:
```bash
git branch
```

The `*` shows your current branch.

View all branches (including remote):
```bash
git branch -a
```

### Step 3: Create Your First Branch

Create a new branch for a feature:
```bash
git branch feature/add-about-section
```

Switch to the new branch:
```bash
git checkout feature/add-about-section
```

Or do both in one command:
```bash
git checkout -b feature/add-about-section
```

Verify you're on the new branch:
```bash
git branch
```

### Step 4: Make Changes on the Branch

Create an about section:
```bash
echo "" >> README.md
echo "## About This Project" >> README.md
echo "This repository demonstrates Git branching concepts." >> README.md
```

Stage and commit:
```bash
git add README.md
git commit -m "Add about section to README"
```

Push the branch to GitHub:
```bash
git push -u origin feature/add-about-section
```

### Step 5: Switch Back to Main

```bash
git checkout main
```

Open README.md - notice the about section is gone! That's because it only exists on the feature branch.

### Step 6: Merge the Feature Branch

Make sure you're on main:
```bash
git checkout main
```

Merge the feature branch:
```bash
git merge feature/add-about-section
```

This performs a "fast-forward" merge. Now main has the about section!

Push to GitHub:
```bash
git push origin main
```

### Step 7: Create and Merge Multiple Branches

**Branch 1: Add Features List**
```bash
git checkout -b feature/add-features
echo "" >> README.md
echo "## Features" >> README.md
echo "- Git branching" >> README.md
echo "- Merging strategies" >> README.md
git add README.md
git commit -m "Add features section"
git push -u origin feature/add-features
```

**Switch back to main and create Branch 2:**
```bash
git checkout main
git checkout -b feature/add-installation
echo "" >> README.md
echo "## Installation" >> README.md
echo "Clone this repository and start learning!" >> README.md
git add README.md
git commit -m "Add installation instructions"
git push -u origin feature/add-installation
```

**Merge both branches:**
```bash
git checkout main
git merge feature/add-features
git merge feature/add-installation
git push origin main
```

### Step 8: Create a Merge Conflict (Intentionally)

**Branch 1:**
```bash
git checkout main
git checkout -b branch-a
echo "Contact: email@example.com" >> README.md
git add README.md
git commit -m "Add contact email"
```

**Branch 2:**
```bash
git checkout main
git checkout -b branch-b
echo "Contact: phone@example.com" >> README.md
git add README.md
git commit -m "Add contact phone"
```

**Create the conflict:**
```bash
git checkout main
git merge branch-a    # This will work fine
git merge branch-b    # This will create a conflict!
```

### Step 9: Resolve the Merge Conflict

Git will tell you there's a conflict. Check status:
```bash
git status
```

Open README.md in your editor. You'll see:
```
<<<<<<< HEAD
Contact: email@example.com
=======
Contact: phone@example.com
>>>>>>> branch-b
```

Edit the file to resolve the conflict:
```
Contact: email@example.com, phone@example.com
```

Stage the resolved file:
```bash
git add README.md
```

Complete the merge:
```bash
git commit -m "Merge branch-b and resolve contact conflict"
git push origin main
```

### Step 10: Clean Up Branches

Delete local branches:
```bash
git branch -d feature/add-about-section
git branch -d feature/add-features
git branch -d feature/add-installation
git branch -d branch-a
git branch -d branch-b
```

Delete remote branches:
```bash
git push origin --delete feature/add-about-section
git push origin --delete feature/add-features
git push origin --delete feature/add-installation
git push origin --delete branch-a
git push origin --delete branch-b
```

## Essential Branch Commands

```bash
# Creating and switching branches
git branch <branch-name>              # Create branch
git checkout <branch-name>            # Switch to branch
git checkout -b <branch-name>         # Create and switch
git switch <branch-name>              # Modern way to switch
git switch -c <branch-name>           # Modern way to create and switch

# Viewing branches
git branch                            # List local branches
git branch -a                         # List all branches
git branch -v                         # Show last commit on each branch
git branch --merged                   # Show merged branches
git branch --no-merged                # Show unmerged branches

# Merging
git merge <branch-name>               # Merge branch into current branch
git merge --no-ff <branch-name>       # Force merge commit
git merge --abort                     # Abort merge after conflict

# Deleting branches
git branch -d <branch-name>           # Delete merged branch
git branch -D <branch-name>           # Force delete branch
git push origin --delete <branch-name> # Delete remote branch
```

## Branching Strategies

### 1. Feature Branch Workflow
```
main
  ├── feature/user-auth
  ├── feature/dashboard
  └── feature/notifications
```
Each feature gets its own branch.

### 2. Git Flow
```
main (production)
  └── develop
      ├── feature/new-feature
      ├── release/v1.0
      └── hotfix/critical-bug
```
More structured with develop, feature, release, and hotfix branches.

### 3. GitHub Flow (Simplified)
```
main
  ├── feature/feature-1
  └── fix/bug-fix
```
Everything branches from and merges to main via pull requests.

## Branch Naming Conventions

Good branch names:
- `feature/add-user-login`
- `fix/null-pointer-exception`
- `docs/update-readme`
- `refactor/optimize-queries`
- `test/add-unit-tests`

Bad branch names:
- `my-branch`
- `test`
- `new-stuff`
- `branch1`

## Practice Exercise

Create a simple website project with branches:

1. **Main branch**: Create basic HTML structure
2. **Feature/header**: Add header section
3. **Feature/content**: Add main content
4. **Feature/footer**: Add footer
5. **Fix/typo**: Fix a typo in content
6. Merge all branches to main
7. Create a conflict and resolve it

## Challenge Tasks

### 1. Three-Way Merge
Create a scenario where three branches need to be merged and ensure clean merging.

### 2. Rebase Instead of Merge
Learn about `git rebase`:
```bash
git checkout feature-branch
git rebase main
```

### 3. Cherry-Pick Commits
Pick specific commits from one branch to another:
```bash
git cherry-pick <commit-hash>
```

### 4. Stash Changes
Save work-in-progress without committing:
```bash
git stash
git stash list
git stash apply
```

### 5. Interactive Rebase
Clean up commit history:
```bash
git rebase -i HEAD~3
```

## Merge Conflict Resolution Tips

1. **Don't Panic**: Conflicts are normal!
2. **Understand Both Sides**: Read what each branch tried to do
3. **Test After Resolving**: Make sure the code still works
4. **Use Tools**: Many editors have built-in conflict resolution tools

**VS Code Conflict Markers:**
```
<<<<<<< HEAD (Current Change)
Your changes
=======
Their changes
>>>>>>> branch-name (Incoming Change)
```

VS Code shows buttons:
- Accept Current Change
- Accept Incoming Change
- Accept Both Changes
- Compare Changes

## Best Practices

1. **Branch Often**: Create a branch for each feature or fix
2. **Keep Branches Small**: Easier to review and merge
3. **Merge Regularly**: Keep your branch up-to-date with main
4. **Delete Merged Branches**: Keep repository clean
5. **Descriptive Names**: Use clear, purpose-driven branch names
6. **One Purpose Per Branch**: Don't mix features and fixes
7. **Pull Before Push**: Always update from remote first

## Common Mistakes to Avoid

❌ Working directly on main branch
✅ Create feature branches

❌ Creating very long-lived branches
✅ Merge frequently to avoid conflicts

❌ Forgetting to push branches to remote
✅ Push branches regularly for backup

❌ Merging without testing
✅ Test before merging

## Expected Output
- Multiple branches created and merged
- Understanding of merge conflicts and resolution
- Clean commit history
- Ability to work on multiple features simultaneously

## Troubleshooting

**Problem**: Can't switch branches (uncommitted changes)
**Solution**: 
```bash
git stash          # Save changes temporarily
git checkout main  # Switch branch
git stash pop      # Restore changes
```

**Problem**: Accidentally committed to wrong branch
**Solution**:
```bash
git log                    # Find commit hash
git checkout correct-branch
git cherry-pick <hash>     # Apply commit to correct branch
git checkout wrong-branch
git reset --hard HEAD~1    # Remove from wrong branch
```

**Problem**: Merge conflict seems overwhelming
**Solution**:
```bash
git merge --abort  # Start over
# Or ask for help!
```

## Next Steps
Continue to [Exercise 6: GitHub Workflow - Creating PRs](06-github-pull-requests.md) to learn about collaborative development!

## Resources
- [Learn Git Branching (Interactive)](https://learngitbranching.js.org/)
- [Atlassian Git Branching Tutorial](https://www.atlassian.com/git/tutorials/using-branches)
- [Git Flow Cheat Sheet](https://danielkummer.github.io/git-flow-cheatsheet/)
- [Understanding Git Merge vs Rebase](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

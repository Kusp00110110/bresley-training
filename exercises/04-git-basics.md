# Exercise 4: Git/VCS Basics - Setup and First Commit

## Overview
Learn the fundamentals of Git version control system, understand why version control is important, and make your first commits.

## Prerequisites
- Git installed on your computer
- A GitHub account
- Basic command line knowledge

## Learning Objectives
- Understand what version control is and why it's important
- Learn essential Git commands
- Configure Git on your local machine
- Create a repository and make commits
- Understand the Git workflow

## What is Version Control?

Version control is a system that records changes to files over time. It allows you to:
- Track every change made to your code
- Revert to previous versions if something breaks
- Collaborate with others without conflicts
- Maintain a history of who changed what and when
- Work on different features simultaneously (branches)

Think of it as "Track Changes" for code, but much more powerful!

## Instructions

### Step 1: Install and Configure Git

First, check if Git is installed:
```bash
git --version
```

If not installed, download from [git-scm.com](https://git-scm.com/)

Configure your identity:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify your configuration:
```bash
git config --list
```

### Step 2: Understanding Git Basics

**Important Git Concepts:**
- **Repository (Repo)**: A project folder tracked by Git
- **Commit**: A snapshot of your project at a specific time
- **Working Directory**: Your current project files
- **Staging Area**: Files ready to be committed
- **Branch**: An independent line of development
- **Remote**: A version of your repo hosted on a server (like GitHub)

**The Git Workflow:**
```
Working Directory → Staging Area → Local Repository → Remote Repository
     (edit)      →   (git add)   →  (git commit)   →   (git push)
```

### Step 3: Create Your First Repository

Create a new project:
```bash
mkdir my-first-repo
cd my-first-repo
```

Initialize Git:
```bash
git init
```

Check status:
```bash
git status
```

### Step 4: Create and Commit a File

Create a README file:
```bash
echo "# My First Git Repository" > README.md
echo "Learning Git basics!" >> README.md
```

Check what changed:
```bash
git status
```

You'll see README.md as "untracked".

Stage the file:
```bash
git add README.md
```

Check status again:
```bash
git status
```

Now README.md is "staged" (ready to commit).

Commit the file:
```bash
git commit -m "Add README with project description"
```

View commit history:
```bash
git log
```

### Step 5: Make More Changes

Edit the README:
```bash
echo "" >> README.md
echo "## What I'm Learning" >> README.md
echo "- Git basics" >> README.md
echo "- Version control concepts" >> README.md
echo "- Committing changes" >> README.md
```

View what changed:
```bash
git diff
```

Stage and commit:
```bash
git add README.md
git commit -m "Add learning objectives to README"
```

### Step 6: Connect to GitHub

Create a new repository on GitHub (don't initialize with README).

Link your local repo to GitHub:
```bash
git remote add origin https://github.com/YOUR-USERNAME/my-first-repo.git
```

Verify remote:
```bash
git remote -v
```

Push your commits to GitHub:
```bash
git branch -M main
git push -u origin main
```

### Step 7: View Your Work on GitHub

1. Go to your repository on GitHub
2. You should see your README.md file
3. Click "Commits" to see your commit history
4. Click on a commit to see what changed

## Essential Git Commands Reference

### Repository Management
```bash
git init                    # Initialize a new Git repository
git clone <url>            # Clone an existing repository
git status                 # Check repository status
```

### Staging and Committing
```bash
git add <file>             # Stage specific file
git add .                  # Stage all changes
git commit -m "message"    # Commit staged changes
git commit -am "message"   # Stage and commit tracked files
```

### Viewing History
```bash
git log                    # View commit history
git log --oneline          # Compact commit history
git log --graph            # Visual commit graph
git diff                   # Show unstaged changes
git diff --staged          # Show staged changes
```

### Working with Remotes
```bash
git remote add origin <url>    # Add remote repository
git remote -v                  # List remotes
git push origin main           # Push to remote
git pull origin main           # Pull from remote
git fetch                      # Download remote changes
```

### Undoing Changes
```bash
git restore <file>         # Discard changes in working directory
git restore --staged <file> # Unstage file
git reset HEAD~1           # Undo last commit (keep changes)
git reset --hard HEAD~1    # Undo last commit (discard changes)
```

## Practice Exercise

Create a new repository called `git-practice`:

1. Initialize a Git repository
2. Create a file called `notes.txt`
3. Add three interesting facts about Git
4. Make three separate commits (one per fact)
5. Create a `.gitignore` file
6. Add `*.log` and `temp/` to `.gitignore`
7. Commit the `.gitignore` file
8. Push everything to GitHub

Example `.gitignore`:
```
# Log files
*.log

# Temporary files
temp/
*.tmp

# OS files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
```

## Challenge Tasks

1. **Commit Message Practice**
   - Make 5 commits with descriptive messages
   - Follow the format: "verb + what changed"
   - Examples: "Add feature", "Fix bug", "Update documentation"

2. **Explore Git Log**
   ```bash
   git log --oneline --graph --all
   git log --author="Your Name"
   git log --since="2 days ago"
   ```

3. **Recover a Deleted File**
   - Delete a file and commit
   - Use `git log` to find the commit before deletion
   - Restore the file from that commit

4. **Interactive Staging**
   - Make multiple changes to different files
   - Use `git add -p` to selectively stage changes

## Best Practices

### Good Commit Messages
✅ **Good:**
- "Add user authentication feature"
- "Fix null pointer exception in login"
- "Update README with installation instructions"

❌ **Bad:**
- "fix"
- "asdf"
- "changed stuff"

### When to Commit
- Commit often (small, logical changes)
- Commit working code
- Don't commit broken code to main branch
- One commit = one logical change

### What NOT to Commit
- Passwords or API keys
- Large binary files
- Dependencies (node_modules, vendor)
- Build artifacts (dist, build)
- Personal IDE settings
- Temporary files

## Expected Output
- Initialized Git repository
- Multiple commits with clear messages
- Files tracked and versioned
- Repository pushed to GitHub
- Understanding of basic Git workflow

## Troubleshooting

**Problem**: `git push` asks for username/password repeatedly
**Solution**: Set up SSH keys or use Personal Access Token

**Problem**: Committed wrong file
**Solution**: 
```bash
git reset HEAD~1         # Undo commit
git restore <file>       # Restore file
```

**Problem**: Forgot to add file to commit
**Solution**:
```bash
git add <forgotten-file>
git commit --amend --no-edit
```

## Next Steps
Continue to [Exercise 5: Git/VCS - Branching and Merging](05-git-branching.md) to learn about working with branches!

## Resources
- [Pro Git Book (Free)](https://git-scm.com/book/en/v2)
- [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Git Visualization Tool](https://git-school.github.io/visualizing-git/)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Common Git mistakes and fixes

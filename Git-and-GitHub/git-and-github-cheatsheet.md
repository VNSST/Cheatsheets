# 🔀 Git & GitHub — Cheatsheet

> A comprehensive reference covering Git fundamentals, branching, collaboration workflows, and GitHub features.

---

## Table of Contents

1. [What is Git?](#what-is-git)
2. [Setup & Configuration](#setup--configuration)
3. [Core Concepts](#core-concepts)
4. [Basic Commands](#basic-commands)
5. [Branching & Merging](#branching--merging)
6. [Remote Repositories](#remote-repositories)
7. [Undoing Changes](#undoing-changes)
8. [Stashing](#stashing)
9. [Git Log & History](#git-log--history)
10. [Tagging](#tagging)
11. [Rebasing](#rebasing)
12. [Cherry-Picking](#cherry-picking)
13. [Git Diff](#git-diff)
14. [.gitignore](#gitignore)
15. [GitHub Features](#github-features)
16. [Pull Requests Workflow](#pull-requests-workflow)
17. [GitHub Actions (CI/CD)](#github-actions-cicd)
18. [SSH & Authentication](#ssh--authentication)
19. [Advanced Git](#advanced-git)
20. [Best Practices](#best-practices)
21. [Quick Reference Card](#quick-reference-card)

---

## What is Git?

Git is a **distributed version control system** that tracks changes to files, enabling collaboration, history tracking, and branching workflows.

```
┌──────────────────────────────────────────────┐
│              Git Architecture                │
│                                              │
│  Working       Staging        Local    Remote│
│  Directory     Area (Index)   Repo     Repo  │
│  ┌─────┐      ┌─────┐      ┌─────┐  ┌─────┐│
│  │     │─add─→│     │─commit→│    │─push→│  ││
│  │     │      │     │       │     │  │     ││
│  │     │←─────────checkout──│     │←pull─│  ││
│  └─────┘      └─────┘      └─────┘  └─────┘│
└──────────────────────────────────────────────┘
```

### Git vs GitHub

| Feature    | Git                          | GitHub                           |
|------------|------------------------------|----------------------------------|
| **What**   | Version control tool         | Cloud hosting platform for Git   |
| **Where**  | Runs locally                 | Web-based service                |
| **Purpose**| Track changes                | Collaborate, share, host repos   |
| **Alternatives** | —                     | GitLab, Bitbucket, Azure DevOps  |

---

## Setup & Configuration

```bash
# Install check
git --version

# Configure identity (required before first commit)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"    # VS Code
git config --global core.editor "vim"             # Vim

# Enable colored output
git config --global color.ui auto

# Set line ending behavior
git config --global core.autocrlf true    # Windows
git config --global core.autocrlf input   # macOS/Linux

# View all settings
git config --list
git config --global --list

# Config levels
git config --system    # System-wide (all users)
git config --global    # Current user (all repos)
git config --local     # Current repo only (default)
```

---

## Core Concepts

### The Three States

| State           | Location         | Description                        |
|-----------------|------------------|------------------------------------|
| **Modified**    | Working Directory| Changed but not staged             |
| **Staged**      | Staging Area     | Marked for next commit             |
| **Committed**   | Local Repository | Safely stored in Git history       |

### Key Terminology

| Term         | Meaning                                              |
|--------------|------------------------------------------------------|
| **Repository (repo)** | Project folder tracked by Git               |
| **Commit**   | Snapshot of changes with a message                   |
| **Branch**   | Independent line of development                      |
| **HEAD**     | Pointer to current branch/commit                     |
| **Remote**   | Copy of repo on a server (e.g., GitHub)              |
| **Origin**   | Default name for the remote repository               |
| **Clone**    | Full copy of a remote repository                     |
| **Fork**     | Personal copy of someone else's repo (GitHub)        |
| **Merge**    | Combine changes from two branches                    |
| **Conflict** | Overlapping changes that Git can't auto-resolve      |
| **Pull**     | Fetch + merge from remote                            |
| **Push**     | Upload local commits to remote                       |

---

## Basic Commands

### Creating Repositories

```bash
# Initialize a new repo in current directory
git init

# Clone an existing repo
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my-folder   # Custom folder name
git clone --depth 1 https://github.com/user/repo.git   # Shallow clone (latest only)
```

### Staging & Committing

```bash
# Check status
git status
git status -s              # Short format

# Stage files
git add file.txt           # Stage specific file
git add file1.txt file2.txt # Stage multiple files
git add .                  # Stage all changes in current dir
git add -A                 # Stage ALL changes (entire repo)
git add *.py               # Stage by pattern
git add -p                 # Interactive: stage parts of files (hunks)

# Unstage (remove from staging, keep changes)
git restore --staged file.txt
git reset HEAD file.txt       # Older syntax

# Commit
git commit -m "Add feature X"
git commit -am "Fix bug Y"    # Stage tracked files + commit (skips git add)
git commit --amend -m "New message"  # Edit last commit message
git commit --amend --no-edit         # Add staged changes to last commit
git commit --allow-empty -m "Empty commit"  # Trigger CI, etc.
```

### Viewing Changes

```bash
# What changed (unstaged)
git diff

# What's staged (ready to commit)
git diff --staged
git diff --cached          # Same as --staged

# Diff between branches
git diff main..feature
git diff main...feature    # Changes since branches diverged

# Diff specific file
git diff file.txt
git diff HEAD~3 file.txt   # File vs 3 commits ago
```

---

## Branching & Merging

### Branch Basics

```bash
# List branches
git branch                 # Local branches
git branch -r              # Remote branches
git branch -a              # All branches (local + remote)
git branch -v              # With last commit info

# Create branch
git branch feature-x       # Create (don't switch)
git checkout -b feature-x  # Create AND switch
git switch -c feature-x    # Create AND switch (modern)

# Switch branches
git checkout main
git switch main             # Modern syntax

# Rename branch
git branch -m old-name new-name
git branch -m new-name     # Rename current branch

# Delete branch
git branch -d feature-x    # Safe delete (must be merged)
git branch -D feature-x    # Force delete
git push origin --delete feature-x  # Delete remote branch
```

### Merging

```bash
# Switch to target branch first
git checkout main

# Merge feature branch into current branch
git merge feature-x

# Merge strategies
git merge feature-x              # Fast-forward if possible
git merge --no-ff feature-x      # Always create merge commit
git merge --squash feature-x     # Squash all commits into one

# Abort a merge (during conflicts)
git merge --abort
```

### Handling Merge Conflicts

```bash
# When conflict occurs, files contain:
<<<<<<< HEAD
Your changes (current branch)
=======
Their changes (incoming branch)
>>>>>>> feature-x

# Resolution steps:
# 1. Edit files to resolve conflicts (remove markers)
# 2. Stage resolved files
git add resolved-file.txt
# 3. Complete the merge
git commit
```

### Branching Strategies

| Strategy          | Description                                    | Best For            |
|-------------------|------------------------------------------------|---------------------|
| **Git Flow**      | main, develop, feature/*, release/*, hotfix/*  | Scheduled releases  |
| **GitHub Flow**   | main + feature branches + PRs                  | Continuous deploy   |
| **Trunk-Based**   | Short-lived branches off main                  | Fast-moving teams   |

#### GitHub Flow (Recommended for most teams)

```
main ─────●────●────●────●────●──── (always deployable)
           \        ↑
feature-x   ●──●──●─┘  (PR → review → merge)
```

---

## Remote Repositories

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git
git remote add upstream https://github.com/original/repo.git

# Remove / rename remote
git remote remove origin
git remote rename origin upstream

# Fetch (download changes without merging)
git fetch origin
git fetch --all              # All remotes
git fetch --prune            # Remove deleted remote branches

# Pull (fetch + merge)
git pull origin main
git pull --rebase origin main  # Rebase instead of merge

# Push
git push origin main
git push -u origin main      # Set upstream (first push)
git push                     # Push to tracked upstream
git push --force             # ⚠️ Force push (rewrites history)
git push --force-with-lease  # Safer force push (checks remote)
git push origin --tags       # Push all tags

# Track remote branch
git checkout --track origin/feature-x
git branch --set-upstream-to=origin/main
```

---

## Undoing Changes

### Levels of Undo

```
Scenario                          │ Command
──────────────────────────────────┼─────────────────────────────────
Discard unstaged changes          │ git restore file.txt
Discard ALL unstaged changes      │ git restore .
Unstage a file                    │ git restore --staged file.txt
Undo last commit (keep changes)   │ git reset --soft HEAD~1
Undo last commit (unstage changes)│ git reset --mixed HEAD~1
Undo last commit (discard all)    │ git reset --hard HEAD~1
Revert a commit (new undo commit) │ git revert <commit-hash>
Edit last commit message          │ git commit --amend -m "new msg"
```

### git reset Modes

```
                Working Dir    Staging Area    History
--soft              ✅ keep       ✅ keep       ❌ undo
--mixed (default)   ✅ keep       ❌ undo       ❌ undo
--hard              ❌ undo       ❌ undo       ❌ undo
```

### git revert vs git reset

| `git revert`                       | `git reset`                       |
|------------------------------------|-----------------------------------|
| Creates a **new** undo commit      | **Removes** commits from history  |
| Safe for shared/pushed branches    | ⚠️ Dangerous on shared branches   |
| Preserves full history             | Rewrites history                  |

```bash
# Revert (safe — creates inverse commit)
git revert abc1234
git revert HEAD          # Revert last commit
git revert HEAD~3..HEAD  # Revert last 3 commits

# Reset (use with caution)
git reset --soft HEAD~1   # Undo commit, keep staged
git reset --hard HEAD~1   # ⚠️ Destroy commit + changes

# Recover from accidental reset
git reflog               # Find the lost commit hash
git reset --hard <hash>  # Restore to that point
```

---

## Stashing

Temporarily save uncommitted changes without committing.

```bash
# Save current changes to stash
git stash
git stash push -m "work in progress on feature X"

# Stash including untracked files
git stash -u
git stash --include-untracked

# List stashes
git stash list
# stash@{0}: On main: work in progress on feature X
# stash@{1}: WIP on main: abc1234 Previous commit message

# Apply stash (keep stash)
git stash apply              # Apply latest
git stash apply stash@{1}    # Apply specific stash

# Apply and remove stash
git stash pop

# Drop a stash
git stash drop stash@{0}

# Clear all stashes
git stash clear

# Create branch from stash
git stash branch new-branch stash@{0}
```

---

## Git Log & History

```bash
# Basic log
git log
git log -n 5                # Last 5 commits
git log --oneline           # Compact one-line format
git log --oneline --graph   # With branch graph
git log --oneline --graph --all  # All branches

# Detailed log
git log --stat              # Show files changed
git log -p                  # Show diffs (patches)
git log --pretty=format:"%h %an %ar - %s"  # Custom format

# Filter log
git log --author="Alice"                # By author
git log --since="2026-01-01"            # By date
git log --after="2 weeks ago"           # Relative date
git log --grep="fix"                    # By commit message
git log -- file.txt                     # For specific file
git log main..feature-x                 # Commits in feature not in main

# Show specific commit
git show abc1234
git show HEAD
git show HEAD~2             # 2 commits before HEAD

# Who changed each line
git blame file.txt
git blame -L 10,20 file.txt  # Lines 10-20 only

# Search for string in history
git log -S "function_name"  # Commits that added/removed string
git log -G "regex_pattern"  # Regex search

# Reference log (all HEAD movements)
git reflog
```

---

## Tagging

Tags mark specific commits (typically releases).

```bash
# Create tags
git tag v1.0.0                          # Lightweight tag
git tag -a v1.0.0 -m "Release v1.0.0"  # Annotated tag (recommended)
git tag -a v1.0.0 abc1234               # Tag a specific commit

# List tags
git tag
git tag -l "v1.*"          # Filter by pattern

# Show tag details
git show v1.0.0

# Push tags
git push origin v1.0.0     # Push specific tag
git push origin --tags      # Push all tags

# Delete tags
git tag -d v1.0.0                    # Delete local
git push origin --delete v1.0.0     # Delete remote

# Checkout a tag
git checkout v1.0.0        # Detached HEAD state
git checkout -b hotfix v1.0.0  # Create branch at tag
```

### Semantic Versioning

```
v MAJOR . MINOR . PATCH
v 1     . 4     . 2

MAJOR — breaking changes
MINOR — new features (backwards compatible)
PATCH — bug fixes (backwards compatible)
```

---

## Rebasing

Reapply commits on top of another branch — creates a linear history.

```bash
# Basic rebase
git checkout feature-x
git rebase main
# Now feature-x starts from latest main

# Interactive rebase (edit history)
git rebase -i HEAD~4       # Last 4 commits

# Interactive rebase commands:
# pick   — keep commit as-is
# reword — change commit message
# edit   — stop to amend commit
# squash — combine with previous commit (keep message)
# fixup  — combine with previous (discard message)
# drop   — remove commit

# Abort rebase
git rebase --abort

# Continue after resolving conflicts
git rebase --continue
```

### Merge vs Rebase

| Aspect           | Merge                       | Rebase                         |
|------------------|-----------------------------|--------------------------------|
| **History**      | Non-linear (merge commits)  | Linear (clean history)         |
| **Safety**       | Safe for shared branches    | ⚠️ Don't rebase shared commits|
| **Conflicts**    | Resolve once                | Resolve per replayed commit    |
| **Use when**     | Preserving branch history   | Cleaning up local commits      |

> ⚠️ **Golden Rule:** Never rebase commits that have been pushed and shared with others.

---

## Cherry-Picking

Apply a specific commit from one branch to another.

```bash
git cherry-pick abc1234              # Apply single commit
git cherry-pick abc1234 def5678      # Multiple commits
git cherry-pick abc1234..xyz9999     # Range of commits
git cherry-pick --no-commit abc1234  # Apply changes without committing

# Abort
git cherry-pick --abort
```

---

## Git Diff

```bash
git diff                       # Unstaged changes
git diff --staged              # Staged changes
git diff HEAD                  # All uncommitted changes
git diff branch1..branch2      # Between branches
git diff abc1234..def5678      # Between commits
git diff --stat                # Summary only
git diff --name-only           # File names only
git diff --word-diff           # Word-level diff
git diff -- path/to/file      # Specific file
```

---

## .gitignore

Tell Git which files/folders to ignore.

```bash
# Create .gitignore in repo root

# Common patterns
*.log                 # All .log files
*.pyc                 # Python compiled files
__pycache__/          # Directory
node_modules/         # Node dependencies
.env                  # Environment variables
.venv/                # Virtual environment
dist/                 # Build output
build/                # Build output
*.exe                 # Executables
.DS_Store             # macOS system file
Thumbs.db             # Windows system file
.idea/                # JetBrains IDE
.vscode/              # VS Code settings (optional)
*.sqlite3             # SQLite databases
coverage/             # Test coverage reports

# Negate (don't ignore)
!important.log

# Nested pattern
**/logs               # 'logs' directory anywhere
```

### Global .gitignore

```bash
# Create a global ignore file
git config --global core.excludesfile ~/.gitignore_global
```

### Already Tracked Files

```bash
# Stop tracking a file (but keep it locally)
git rm --cached file.txt
git rm -r --cached folder/

# Then add to .gitignore and commit
```

---

## GitHub Features

### Repository Features

| Feature            | Description                                       |
|--------------------|---------------------------------------------------|
| **Issues**         | Bug reports, feature requests, task tracking      |
| **Pull Requests**  | Propose, review, and merge changes                |
| **Actions**        | CI/CD automation workflows                        |
| **Pages**          | Free static website hosting from repo             |
| **Wiki**           | Documentation wiki for projects                   |
| **Projects**       | Kanban boards for project management              |
| **Releases**       | Packaged versions with release notes              |
| **Discussions**    | Q&A and community forum for repos                 |
| **Codespaces**     | Cloud dev environments                            |
| **Copilot**        | AI-powered code assistant                         |

### GitHub CLI (`gh`)

```bash
# Install: https://cli.github.com
gh auth login

# Repos
gh repo create my-project --public
gh repo clone user/repo
gh repo fork user/repo

# Pull Requests
gh pr create --title "Add feature" --body "Description"
gh pr list
gh pr checkout 42
gh pr merge 42
gh pr review 42 --approve

# Issues
gh issue create --title "Bug report" --label "bug"
gh issue list
gh issue close 15

# Actions
gh run list
gh run view 12345
```

### Forking Workflow (Open Source)

```bash
# 1. Fork the repo on GitHub (click "Fork" button)

# 2. Clone YOUR fork
git clone https://github.com/YOUR-USER/repo.git

# 3. Add original repo as "upstream"
git remote add upstream https://github.com/ORIGINAL/repo.git

# 4. Create feature branch
git checkout -b my-feature

# 5. Make changes, commit, push to YOUR fork
git push origin my-feature

# 6. Create Pull Request on GitHub (your fork → original repo)

# 7. Keep fork in sync
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Pull Requests Workflow

### Creating a Good PR

```markdown
## PR Title: Add user authentication module

### Description
Implements JWT-based authentication with login/signup endpoints.

### Changes
- Added `auth/` module with login, signup, and token refresh
- Added middleware for route protection
- Added unit tests (95% coverage)

### Testing
- [ ] Unit tests pass
- [ ] Manual testing on staging
- [ ] No breaking changes

### Screenshots
(if UI changes)

### Related Issues
Closes #42, Relates to #38
```

### Code Review Best Practices

```
As a Reviewer:
  ✅ Be constructive and kind
  ✅ Focus on logic, security, and maintainability
  ✅ Suggest, don't demand (unless critical)
  ✅ Approve when "good enough" — don't block on style

As an Author:
  ✅ Keep PRs small and focused (< 400 lines ideal)
  ✅ Write descriptive titles and descriptions
  ✅ Self-review before requesting review
  ✅ Respond to all comments
```

---

## GitHub Actions (CI/CD)

### Basic Workflow

```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest --verbose

      - name: Lint
        run: |
          pip install ruff
          ruff check .
```

### Common Triggers

```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * 1'      # Every Monday midnight
  workflow_dispatch:           # Manual trigger
  release:
    types: [published]
```

### Useful Actions

| Action                     | Purpose                    |
|----------------------------|----------------------------|
| `actions/checkout@v4`      | Clone repo                 |
| `actions/setup-python@v5`  | Install Python             |
| `actions/setup-node@v4`    | Install Node.js            |
| `actions/cache@v4`         | Cache dependencies         |
| `actions/upload-artifact@v4`| Upload build artifacts    |
| `peaceiris/actions-gh-pages`| Deploy to GitHub Pages   |

---

## SSH & Authentication

### Set Up SSH Key

```bash
# 1. Generate SSH key
ssh-keygen -t ed25519 -C "you@example.com"

# 2. Start SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Copy public key
cat ~/.ssh/id_ed25519.pub
# Then paste in GitHub → Settings → SSH and GPG keys → New SSH Key

# 4. Test connection
ssh -T git@github.com
# "Hi username! You've successfully authenticated..."

# 5. Use SSH URLs for repos
git clone git@github.com:user/repo.git
git remote set-url origin git@github.com:user/repo.git
```

### HTTPS vs SSH

| Method   | URL Format                              | Auth Method          |
|----------|-----------------------------------------|----------------------|
| **HTTPS**| `https://github.com/user/repo.git`     | Token / credential manager |
| **SSH**  | `git@github.com:user/repo.git`         | SSH key pair         |

---

## Advanced Git

### Git Bisect (Find Bug-Introducing Commit)

```bash
git bisect start
git bisect bad               # Current commit is broken
git bisect good abc1234      # This old commit was working

# Git checks out middle commit — test it, then:
git bisect good              # If this commit works
git bisect bad               # If this commit is broken

# Repeat until Git finds the exact commit
git bisect reset             # Return to original state
```

### Git Worktrees (Multiple Working Directories)

```bash
# Work on multiple branches simultaneously without switching
git worktree add ../hotfix hotfix-branch
git worktree list
git worktree remove ../hotfix
```

### Git Submodules

```bash
# Add a submodule
git submodule add https://github.com/user/lib.git libs/lib

# Clone repo with submodules
git clone --recurse-submodules https://github.com/user/repo.git

# Update submodules
git submodule update --init --recursive
```

### Git Hooks

Scripts that run automatically on Git events.

```
.git/hooks/
├── pre-commit       # Before commit (lint, format)
├── commit-msg       # Validate commit message
├── pre-push         # Before push (run tests)
├── post-merge       # After merge (install deps)
└── pre-rebase       # Before rebase
```

```bash
# Example pre-commit hook (.git/hooks/pre-commit)
#!/bin/sh
echo "Running linter..."
ruff check . || exit 1
echo "Lint passed ✅"
```

### Aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.last "log -1 HEAD"
git config --global alias.unstage "restore --staged"

# Usage
git co main
git lg
git st
```

---

## Best Practices

### Commit Messages

```
Format:
  <type>: <short summary>     (50 chars max)

  <optional body>              (wrap at 72 chars)
  <optional footer>

Types:
  feat:     New feature
  fix:      Bug fix
  docs:     Documentation
  style:    Formatting (no code change)
  refactor: Code restructuring
  test:     Adding/fixing tests
  chore:    Build, config, dependencies

Examples:
  feat: add user login with JWT authentication
  fix: resolve null pointer in payment processing
  docs: update API reference for v2 endpoints
  refactor: extract validation logic into separate module
```

### General Best Practices

```
✅ Commit often with clear messages
✅ Pull before you push
✅ Use feature branches (never commit directly to main)
✅ Keep branches short-lived
✅ Write meaningful .gitignore early
✅ Review your own diff before committing (git diff --staged)
✅ Use PRs for all changes to main
✅ Tag releases with semantic versioning
✅ Don't commit secrets, credentials, or .env files

❌ Don't force push to shared branches
❌ Don't commit large binary files (use Git LFS)
❌ Don't rebase published/shared commits
❌ Don't commit generated files (node_modules, __pycache__)
```

---

## Quick Reference Card

```
┌───────────────────────────────────────────────────┐
│              GIT QUICK REFERENCE                  │
├───────────────────────────────────────────────────┤
│ git init                    Initialize repo       │
│ git clone <url>             Clone repo            │
│ git status                  Check status          │
│ git add .                   Stage all             │
│ git commit -m "msg"         Commit                │
│ git push origin main        Push to remote        │
│ git pull origin main        Pull from remote      │
│ git branch feature          Create branch         │
│ git switch feature          Switch branch         │
│ git switch -c feature       Create & switch       │
│ git merge feature           Merge branch          │
│ git log --oneline --graph   View history          │
│ git stash                   Save work temporarily │
│ git stash pop               Restore stashed work  │
│ git diff                    View changes          │
│ git restore file.txt        Discard changes       │
│ git reset --soft HEAD~1     Undo last commit      │
│ git revert <hash>           Reverse a commit      │
│ git remote -v               List remotes          │
│ git tag -a v1.0 -m "msg"    Create tag            │
│ git rebase main             Rebase onto main      │
│ git cherry-pick <hash>      Apply specific commit │
│ git blame file.txt          Who changed what      │
│ git reflog                  Recovery lifeline     │
└───────────────────────────────────────────────────┘
```

---

> **Last Updated:** April 2026

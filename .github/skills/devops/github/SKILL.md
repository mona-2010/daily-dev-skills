---
name: github
description: "Comprehensive guide to using Git and GitHub for version control, collaboration, and repository management. Use when: setting up Git for the first time; learning Git commands; managing branches and pull requests; following professional Git workflows; using GitHub features effectively."
user-invocable: true
---

# Git & GitHub Complete Guide

A practical production-grade guide for developers to manage code, collaborate with teams, maintain repositories, and follow professional Git workflows from local setup to pull request merging.

---

# What is Git?

Git is a distributed version control system used to:

* track code changes
* collaborate with teams
* manage project history
* create branches safely
* deploy software workflows

---

# What is GitHub?

GitHub is a cloud-based platform built around Git that provides:

* repository hosting
* pull requests
* collaboration
* CI/CD integrations
* issue tracking
* project management

---

# Install Git

Download Git:

[Git Official Website](https://git-scm.com/downloads?utm_source=chatgpt.com)

Verify installation:

```bash id="jlwm1"
git --version
```

---

# Configure Git

Set username:

```bash id="’wini2"
git config --global user.name "Your Name"
```

Set email:

```bash id="’wini3"
git config --global user.email "your@email.com"
```

Check config:

```bash id="’wini4"
git config --list
```

---

# Create GitHub Account

Create account:

[GitHub Signup](https://github.com/signup?utm_source=chatgpt.com)

---

# Generate SSH Key (Recommended)

Generate key:

```bash id="’wini5"
ssh-keygen -t ed25519 -C "your@email.com"
```

Copy key:

```bash id="’wini6"
cat ~/.ssh/id_ed25519.pub
```

Add it to:

```txt id="’wini7"
GitHub → Settings → SSH and GPG Keys
```

Test connection:

```bash id="’wini8"
ssh -T git@github.com
```

---

# Initialize Git Repository

Inside project:

```bash id="’wini9"
git init
```

This creates:

```txt id="’wini10"
.git/
```

which tracks version history.

---

# Connect Local Project to GitHub

Create repository on GitHub.

Then connect:

```bash id="’wini11"
git remote add origin https://github.com/username/repository.git
```

Verify:

```bash id="’wini12"
git remote -v
```

---

# Basic Git Workflow

```txt id="’wini13"
Code Changes
     ↓
git add
     ↓
git commit
     ↓
git push
     ↓
Create Pull Request
     ↓
Code Review
     ↓
Merge
```

---

# Check File Status

```bash id="’wini14"
git status
```

Shows:

* modified files
* staged files
* untracked files

---

# Stage Files

Stage specific file:

```bash id="’wini15"
git add file.ts
```

Stage all files:

```bash id="’wini16"
git add .
```

---

# Create Commit

```bash id="’wini17"
git commit -m "feat: add user authentication flow"
```

A commit should represent:

* one logical change
* clean purpose
* understandable history

---

# Professional Commit Message Standards

Recommended format:

```txt id="’wini18"
type: short description
```

---

# Common Commit Types

| Type     | Purpose                 |
| -------- | ----------------------- |
| feat     | new feature             |
| fix      | bug fix                 |
| refactor | code restructuring      |
| docs     | documentation           |
| style    | formatting              |
| test     | testing                 |
| chore    | maintenance             |
| perf     | performance improvement |

---

# Good Commit Examples

```bash id="’wini19"
git commit -m "feat: implement role-based access control"
```

```bash id="’wini20"
git commit -m "fix: resolve dashboard loading issue"
```

```bash id="’wini21"
git commit -m "refactor: separate API logic into services"
```

---

# Bad Commit Examples

❌

```txt id="’wini22"
update
changes
fixed stuff
final
```

These provide no useful context.

---

# Push Code to GitHub

First push:

```bash id="’wini23"
git push -u origin main
```

Later pushes:

```bash id="’wini24"
git push
```

---

# Pull Latest Changes

```bash id="’wini25"
git pull origin main
```

Always pull latest changes before starting work.

---

# Branching Strategy

Never work directly on:

```txt id="’wini26"
main
```

Create feature branches.

---

# Create Branch

```bash id="’wini27"
git checkout -b feature/user-authentication
```

---

# Switch Branches

```bash id="’wini28"
git checkout branch-name
```

---

# Recommended Branch Naming

## Features

```txt id="’wini29"
feature/user-authentication
feature/payment-integration
```

## Bug Fixes

```txt id="’wini30"
fix/login-validation
fix/navbar-overflow
```

## Refactors

```txt id="’wini31"
refactor/api-structure
```

---

# Push New Branch

```bash id="’wini32"
git push -u origin feature/user-authentication
```

---

# Create Pull Request (PR)

After pushing branch:

1. Open GitHub repository
2. Click:

   ```txt id="’wini33"
   Compare & Pull Request
   ```
3. Add:

   * title
   * description
   * screenshots (if UI)
   * testing notes

---

# Professional PR Title Examples

Good:

```txt id="’wini34"
feat: implement manager onboarding workflow
```

```txt id="’wini35"
fix: resolve employee table pagination issue
```

Bad:

```txt id="’wini36"
changes
updated code
final fix
```

---

# Professional PR Description Template

```md id="’wini37"
## Changes
- added onboarding flow
- implemented validation
- fixed API handling

## Testing
- tested locally
- verified responsive layout

## Screenshots
(optional)
```

---

# Recommended PR Workflow

```txt id="’wini38"
Feature Branch
      ↓
Push Branch
      ↓
Create PR
      ↓
Code Review
      ↓
Address Feedback
      ↓
Merge
```

---

# Review & Merge PR

After approval:

* squash commits if needed
* merge into development branch
* delete old branch

---

# Delete Local Branch

```bash id="’wini39"
git branch -d feature/user-authentication
```

---

# Delete Remote Branch

```bash id="’wini40"
git push origin --delete feature/user-authentication
```

---

# View Commit History

```bash id="’wini41"
git log
```

Short version:

```bash id="’wini42"
git log --oneline
```

---

# Undo Changes

## Discard unstaged changes

```bash id="’wini43"
git checkout -- file.ts
```

---

## Unstage file

```bash id="’wini44"
git reset HEAD file.ts
```

---

## Reset last commit

```bash id="’wini45"
git reset --soft HEAD~1
```

---

# Git Stash

Temporarily save work:

```bash id="’wini46"
git stash
```

Restore:

```bash id="’wini47"
git stash pop
```

Useful when switching tasks quickly.

---

# Merge Branches

```bash id="’wini48"
git checkout main
git merge feature/user-authentication
```

---

# Resolve Merge Conflicts

Git conflict markers:

```txt id="’wini49"
<<<<<<< HEAD
=======
>>>>>>> branch
```

Steps:

1. resolve manually
2. remove markers
3. add files
4. commit changes

---

# .gitignore Best Practices

Create:

```txt id="’wini50"
.gitignore
```

Common entries:

```txt id="’wini51"
node_modules/
.env
.next/
dist/
coverage/
```

Never commit:

* secrets
* environment files
* build files
* dependencies

---

# GitHub Best Practices

# 1. Keep Repositories Clean

Maintain:

* proper README
* folder structure
* documentation
* issue tracking

---

# 2. Use README Properly

README should include:

* project overview
* tech stack
* installation steps
* screenshots
* features

---

# 3. Protect Main Branch

Enterprise teams protect:

* main
* production

Prevent direct pushes.

---

# 4. Use Pull Requests for Everything

Avoid direct commits to production branches.

---

# 5. Write Meaningful Commit Messages

Good history improves:

* debugging
* collaboration
* onboarding

---

# GitHub Actions (CI/CD)

Automate:

* testing
* linting
* deployments

Location:

```txt id="’wini52"
.github/workflows/
```

---

# Example Workflow

```yaml id="’wini53"
name: CI

on:
  push:
    branches:
      - main
```

---

# Fork vs Clone

| Action | Purpose              |
| ------ | -------------------- |
| clone  | download repo        |
| fork   | create personal copy |

---

# Clone Repository

```bash id="’wini54"
git clone https://github.com/username/repository.git
```

---

# Fetch vs Pull

| Command | Purpose          |
| ------- | ---------------- |
| fetch   | download changes |
| pull    | fetch + merge    |

---

# Rebase vs Merge

## Merge

Preserves history.

## Rebase

Creates cleaner linear history.

Recommended for experienced teams.

---

# Recommended Enterprise Workflow

```txt id="’wini55"
main
│
├── dev
│
├── feature/*
├── fix/*
└── hotfix/*
```

---

# Common Git Mistakes

# ❌ Working Directly on Main

Always use branches.

---

# ❌ Poor Commit Messages

Avoid meaningless history.

---

# ❌ Huge PRs

Smaller PRs are easier to review.

---

# ❌ Committing Secrets

Never commit:

* .env
* API keys
* credentials

---

# ❌ Ignoring Pull Before Push

Can cause conflicts.

---

# ❌ Mixing Multiple Features in One PR

Keep PRs focused.

---

# Recommended Developer Workflow

```txt id="’wini56"
Pull Latest Code
      ↓
Create Branch
      ↓
Develop Feature
      ↓
Commit Properly
      ↓
Push Branch
      ↓
Create PR
      ↓
Code Review
      ↓
Merge
```

---

# Recommended Git Tools

| Tool           | Purpose                |
| -------------- | ---------------------- |
| GitHub Desktop | GUI Git management     |
| Sourcetree     | Git visualization      |
| VS Code Git    | integrated Git support |

---

# Final Advice For Developers

Git is not just about saving code.

It is about:

* collaboration
* code quality
* maintainability
* engineering workflows
* team productivity

Focus on:

* clean commits
* meaningful PRs
* proper branching
* disciplined workflows

Good Git practices separate professional engineers from beginners.

---

# Key Takeaways

* Use feature branches
* Write professional commit messages
* Keep PRs focused
* Never commit secrets
* Use meaningful Git workflows
* Maintain clean repository history
* Use pull requests for collaboration
* Follow scalable team workflows

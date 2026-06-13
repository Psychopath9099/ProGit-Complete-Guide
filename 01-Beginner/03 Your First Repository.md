---
title: Your First Repository
tags: [git, beginner, repository, init, clone]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-init
related:
  - "[[04 The Three States]]"
  - "[[05 Staging and Committing]]"
  - "[[08 Working with Remotes]]"
---

# Your First Repository

## Overview

A **repository** (repo) is a folder being tracked by Git. It contains your files plus a hidden `.git/` subdirectory where Git stores its entire database — every snapshot, every branch, every piece of history.

There are two ways to get a repository:
1. **`git init`** — create a new repo in an existing folder
2. **`git clone`** — copy an existing repo from somewhere else

---

## Method 1: `git init` — Start Fresh

### Initialize a new project

```bash
# Create a new folder and initialize it
mkdir my-project
cd my-project
git init
# Initialized empty Git repository in /home/user/my-project/.git/
```

### Initialize an existing folder
```bash
cd ~/existing-project
git init
# Initialized empty Git repository in .git/
```

Git creates a `.git/` directory. **Don't touch this folder** — it's Git's internal database. Deleting it removes all of Git's history for your project (your files stay, but all version tracking is gone).

```bash
ls -la
# .git/     ← Git's database (hidden)
# README.md
# src/
```

### Your first commit after init
```bash
git init
git add .
git commit -m "Initial commit"
```

---

## Method 2: `git clone` — Copy an Existing Repo

`git clone` downloads a repository from a URL. It:
- Creates a new directory with the project name
- Downloads the entire history (every commit, every branch)
- Sets up a `remote` called `origin` pointing to the source
- Checks out the default branch

```bash
# Clone from GitHub
git clone https://github.com/username/repo-name

# This creates: ./repo-name/
```

### Clone into a specific folder name
```bash
git clone https://github.com/libgit2/libgit2 mylib
# Creates: ./mylib/
```

### Clone via SSH (if you have SSH keys set up)
```bash
git clone git@github.com:username/repo-name.git
```

> [!tip] HTTPS vs SSH
> - **HTTPS:** Easier to set up initially. You'll be asked for credentials when pushing.
> - **SSH:** Requires one-time key setup, but never asks for passwords. Preferred for regular use.

---

## The `.git/` Directory

When you run `git init` or `git clone`, Git creates a `.git/` folder:

```
.git/
├── config          ← repo-level git config
├── HEAD            ← which branch you're on
├── index           ← the staging area
├── objects/        ← all your commits, files, trees
└── refs/
    ├── heads/      ← local branches
    └── remotes/    ← remote tracking branches
```

> [!note] Advanced
> This structure is explained in detail in [[01 Git Object Model]] and [[03 Refs and HEAD]]. You don't need to understand it yet, but knowing it exists demystifies many Git behaviors.

---

## Checking Your Repository Status

After initializing or cloning, always check the status:

```bash
git status
# On branch main
# nothing to commit, working tree clean
```

This is the most useful command in Git. Run it constantly.

---

## Practical Walkthrough: New Project

```bash
# 1. Create project
mkdir my-app
cd my-app

# 2. Initialize Git
git init

# 3. Create some files
echo "# My App" > README.md
echo "console.log('hello')" > app.js

# 4. See what Git sees
git status
# Untracked files: README.md, app.js

# 5. Stage all files
git add .

# 6. Commit
git commit -m "Initial commit: add README and app.js"

# 7. Verify
git log --oneline
# a3f1b2c Initial commit: add README and app.js
```

---

## Practical Walkthrough: Cloning

```bash
# 1. Clone a repo
git clone https://github.com/octocat/Hello-World
cd Hello-World

# 2. See the remote is already set up
git remote -v
# origin  https://github.com/octocat/Hello-World (fetch)
# origin  https://github.com/octocat/Hello-World (push)

# 3. See all branches
git branch -a
# * main
#   remotes/origin/main
```

---

## Common Mistakes

> [!bug] `git init` inside Dropbox/Google Drive
> Don't put Git repos inside sync folders. The `.git/` database is not designed to be synced in real-time by file sync tools. Conflicts will corrupt it.

> [!bug] Accidentally initializing in home directory
> Running `git init` in `~` makes your entire home directory a repo. If you did this:
> ```bash
> cd ~
> ls -la | grep .git   # check if it exists
> rm -rf .git          # remove it (removes tracking only, not your files)
> ```

> [!bug] `git clone` vs download ZIP
> Downloading a ZIP from GitHub gives you the files but **not the Git history**. Always use `git clone` if you want to contribute back or have version tracking.

---

## Exercises

1. Create a new folder, initialize a Git repo, create 3 files, and make your first commit.
2. Clone any public GitHub repository and inspect its commit history with `git log --oneline`.
3. Inspect the `.git/` directory structure with `ls -la .git/`.

---

## Related Notes

- [[04 The Three States]] — What happens between init and commit
- [[05 Staging and Committing]] — The full commit workflow
- [[08 Working with Remotes]] — Pushing and pulling

---

## Official References

- [`git init` docs](https://git-scm.com/docs/git-init)
- [`git clone` docs](https://git-scm.com/docs/git-clone)
- [Pro Git: Getting a Git Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)

---

#git/beginner #git/repository #git/init #git/clone

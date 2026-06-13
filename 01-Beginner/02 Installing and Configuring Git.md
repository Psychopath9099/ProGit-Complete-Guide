---
title: Installing and Configuring Git
tags: [git, beginner, setup, config]
type: setup-note
difficulty: beginner
official-doc: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
related:
  - "[[01 What is Git]]"
  - "[[03 Your First Repository]]"
---

# Installing and Configuring Git

## Overview

Before using Git, you need to install it and tell it who you are. This identity information gets baked into every commit you make — it's how collaborators know who changed what.

---

## Installation

### Linux (Debian/Ubuntu)
```bash
sudo apt install git-all
```

### Linux (Fedora/RHEL/CentOS)
```bash
sudo dnf install git-all
```

### macOS
Try running `git --version` in Terminal. If Git isn't installed, macOS will prompt you to install Xcode Command Line Tools, which includes Git.

Or install a newer version via Homebrew:
```bash
brew install git
```

### Windows
Download the installer from [git-scm.com/download/win](https://git-scm.com/download/win). Use all defaults unless you know what you're changing. This installs "Git Bash", a terminal that works like Linux.

### Verify Installation
```bash
git --version
# git version 2.47.0
```

---

## First-Time Setup

Git uses `git config` to store settings. Settings live in three places, each overriding the previous:

| Level | File Location | Flag | Scope |
|---|---|---|---|
| System | `/etc/gitconfig` | `--system` | All users on this machine |
| Global | `~/.gitconfig` | `--global` | You, across all repos |
| Local | `.git/config` | `--local` | This specific repo |

### Step 1: Set Your Identity

**This is required.** Every commit you make uses this information.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

> [!warning] Use a real email
> This email will be embedded in every commit. If you push to GitHub, it should match your GitHub email (or use GitHub's privacy email).

### Step 2: Set Your Default Editor

Git opens a text editor for commit messages when you don't use `-m`. Set your preferred editor:

```bash
# VS Code
git config --global core.editor "code --wait"

# Neovim
git config --global core.editor "nvim"

# nano (simplest for beginners)
git config --global core.editor "nano"
```

### Step 3: Set the Default Branch Name

Modern convention uses `main` instead of `master`:

```bash
git config --global init.defaultBranch main
```

### Step 4: Set Line Ending Handling (optional but recommended)

```bash
# macOS/Linux
git config --global core.autocrlf input

# Windows
git config --global core.autocrlf true
```

This prevents Windows line endings (`\r\n`) from polluting your commits.

---

## Verify Your Config

```bash
# See all settings and where they come from
git config --list --show-origin

# Check a specific value
git config user.name
git config user.email
```

---

## Your `~/.gitconfig` File

After setup, your global config file looks something like:

```ini
[user]
    name = Your Name
    email = you@example.com
[core]
    editor = code --wait
    autocrlf = input
[init]
    defaultBranch = main
[alias]
    st = status
    lg = log --oneline --graph --all
```

You can edit this file directly in any text editor.

---

## Useful Config Additions

### Better `git log` by default
```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
```

### Color output
```bash
git config --global color.ui auto
```

### Push behavior
```bash
# Only push the current branch (safer)
git config --global push.default current
```

### Set a global .gitignore (ignore junk files everywhere)
```bash
git config --global core.excludesfile ~/.gitignore_global
```

Then create `~/.gitignore_global`:
```
.DS_Store
Thumbs.db
*.log
.env
node_modules/
```

---

## Getting Help

```bash
# Full manual page for any command
git help config
git help commit

# Short flag summary
git config -h
git add -h
```

---

## Common Mistakes

> [!bug] Forgot to set identity
> If you commit without setting `user.name` and `user.email`, Git will use defaults from your system that may be ugly or wrong. Set them first!

> [!bug] Wrong email for GitHub
> If your `user.email` doesn't match a GitHub account email, contributions won't show up in your GitHub activity graph. Fix it with `git config --global user.email "correct@email.com"`.

---

## Related Notes

- [[01 What is Git]] — Background
- [[03 Your First Repository]] — Now let's use it
- [[12 Git Aliases]] — More config tricks

---

## Official References

- [Pro Git: First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
- [`git config` documentation](https://git-scm.com/docs/git-config)

---

#git/beginner #git/setup #git/config

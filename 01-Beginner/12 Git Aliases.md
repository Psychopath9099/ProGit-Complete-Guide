---
title: Git Aliases and Config
tags: [git, beginner, config, aliases, productivity]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-config
related:
  - "[[02 Installing and Configuring Git]]"
---

# Git Aliases and Config

## Overview

Git aliases let you create shortcuts for long commands. A well-configured Git environment makes daily work dramatically faster. This note covers the most useful aliases and config settings.

---

## Creating Aliases

```bash
git config --global alias.<shortcut> "<command>"
```

### Essential Aliases

```bash
# Status
git config --global alias.st status

# Short log
git config --global alias.lg "log --oneline --graph --all --decorate"

# Last 5 commits
git config --global alias.last "log -5 --oneline"

# Quick diff of staged changes
git config --global alias.staged "diff --staged"

# Undo last commit, keep changes staged
git config --global alias.undo "reset --soft HEAD~1"

# Branches verbose
git config --global alias.branches "branch -vv"

# List all aliases
git config --global alias.aliases "config --get-regexp alias"
```

Now use them:
```bash
git st          # git status
git lg          # beautiful log
git undo        # undo last commit
```

### More Advanced Aliases

```bash
# Show contributors
git config --global alias.contributors "shortlog -sn"

# Show what changed in last commit
git config --global alias.wtf "show --stat"

# Stash including untracked
git config --global alias.save "stash push -u -m"

# Delete all merged branches (cleanup)
git config --global alias.cleanup "!git branch --merged | grep -v '*\|main\|master\|develop' | xargs -n 1 git branch -d"
```

---

## Your `~/.gitconfig` — Full Recommended Setup

```ini
[user]
    name = Your Name
    email = you@example.com

[core]
    editor = code --wait        # VS Code; change to nvim, nano, etc.
    autocrlf = input            # Linux/Mac; use 'true' on Windows
    excludesfile = ~/.gitignore_global

[init]
    defaultBranch = main

[push]
    default = current           # Push current branch by default

[pull]
    rebase = true               # git pull uses rebase instead of merge

[merge]
    tool = vscode
    conflictstyle = diff3       # Shows base/ours/theirs in conflicts

[diff]
    colorMoved = default

[color]
    ui = auto

[alias]
    st   = status
    co   = checkout
    sw   = switch
    br   = branch
    lg   = log --oneline --graph --all --decorate
    last = log -5 --oneline
    undo = reset --soft HEAD~1
    staged = diff --staged
    aa   = add --all
    wip  = "!git add -A && git commit -m 'WIP: work in progress'"

[rerere]
    enabled = true              # Remember conflict resolutions
```

---

## Useful Config Settings Explained

### `pull.rebase = true`
```bash
git config --global pull.rebase true
```
Makes `git pull` use rebase instead of creating a merge commit. Keeps history cleaner.

### `rerere.enabled = true`
```bash
git config --global rerere.enabled true
```
"Reuse Recorded Resolution" — Git remembers how you resolved a conflict and applies it automatically next time. Invaluable for long-running branches.

### `push.default = current`
```bash
git config --global push.default current
```
Pushes the current branch to its same-named remote branch. Safer and more intuitive than `matching` (which pushes all matching branches).

### `diff.colorMoved = default`
```bash
git config --global diff.colorMoved default
```
Highlights moved code differently from changed code in diffs. Very helpful.

---

## Viewing Config

```bash
# See all settings
git config --list

# See where each setting comes from
git config --list --show-origin

# Get a specific value
git config user.email
git config alias.lg
```

---

## Config Scope

```bash
# System-wide (all users on machine) — needs sudo
git config --system <key> <value>

# Your user (all your repos)
git config --global <key> <value>

# This repo only
git config --local <key> <value>   # default scope
```

Local settings override global, which override system.

---

## Shell Aliases (Even Faster)

In your `~/.bashrc` or `~/.zshrc`:

```bash
# Uber-short aliases
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gpl='git pull'
alias gl='git lg'            # uses your git alias
alias gsw='git switch'
alias gb='git branch'
```

Then:
```bash
gs          # git status
ga .        # git add .
gc "fix bug"  # git commit -m "fix bug"
```

---

## Related Notes

- [[02 Installing and Configuring Git]] — Initial setup
- [[📋 Master Cheat Sheet]] — Quick reference

---

## Official References

- [`git config` docs](https://git-scm.com/docs/git-config)
- [Pro Git: Git Aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)

---

#git/beginner #git/config #git/aliases #git/productivity

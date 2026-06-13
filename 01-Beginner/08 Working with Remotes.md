---
title: Working with Remotes
tags: [git, beginner, remote, push, pull, fetch, github]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-remote
related:
  - "[[03 Your First Repository]]"
  - "[[09 Branching Basics]]"
  - "[[10 Team Workflows]]"
---

# Working with Remotes

## Overview

A **remote** is a version of your repository hosted somewhere else — typically on GitHub, GitLab, or Bitbucket. Remotes enable collaboration and serve as your backup. The most common remote is called `origin` by convention.

---

## Core Concepts

```
Your Machine                     GitHub (or other)
─────────────────                ─────────────────
local repo (origin/main) ←────→  remote repo (main)
         ↑
     git push  → sends your commits up
     git fetch → downloads remote commits
     git pull  → fetch + merge in one step
```

---

## Viewing Remotes

```bash
# List all remotes
git remote

# List remotes with their URLs
git remote -v

# Example output:
# origin  https://github.com/you/your-repo.git (fetch)
# origin  https://github.com/you/your-repo.git (push)
```

---

## Adding a Remote

When you `git clone`, a remote called `origin` is created automatically. For a repo you created with `git init`, you add one manually:

```bash
git remote add origin https://github.com/you/your-repo.git

# Verify
git remote -v
```

The name `origin` is a convention — you can name it anything, but `origin` is universal.

---

## `git fetch` — Download Without Merging

`git fetch` downloads all new commits, branches, and tags from the remote into your local repository — **without changing your working tree or current branch**.

```bash
git fetch origin
git fetch         # fetches from 'origin' by default
```

After fetching, you can inspect what's new:
```bash
git log origin/main --oneline   # see what's on the remote
git diff main origin/main       # see differences
```

Then merge when you're ready:
```bash
git merge origin/main
```

> [!tip] Fetch before you merge
> `git fetch` is always safe — it never changes your work. Use it to see what's changed before deciding to merge.

---

## `git pull` — Fetch + Merge

`git pull` is a shortcut for `git fetch` followed by `git merge`:

```bash
git pull
git pull origin main   # explicit
```

**What happens:**
1. Downloads new commits from remote
2. Merges them into your current branch

```bash
# Pull with rebase instead of merge (cleaner history)
git pull --rebase
```

> [!tip] `git pull --rebase` is often preferred
> Instead of creating a merge commit, it replays your local commits on top of the fetched commits. This keeps a cleaner, linear history.
> Set it as default: `git config --global pull.rebase true`

---

## `git push` — Upload Your Commits

```bash
# Push current branch to its remote counterpart
git push

# Push to a specific remote and branch
git push origin main

# First push of a new branch (set upstream)
git push -u origin feature-branch
# -u sets the upstream tracking relationship
# After this, just `git push` works for this branch
```

### What is "upstream"?

When you create a local branch and push it for the first time, you need to tell Git *where* to push:

```bash
git switch -c new-feature    # create local branch
git push -u origin new-feature   # push + set upstream

# Now Git knows: "new-feature" tracks "origin/new-feature"
# Future pushes: just type `git push`
```

---

## Setting Up a New Repo on GitHub: Full Workflow

```bash
# 1. Create repo on GitHub (empty — no README, .gitignore, or license)

# 2. Initialize locally
git init
git add .
git commit -m "Initial commit"

# 3. Connect to GitHub
git remote add origin https://github.com/you/your-repo.git

# 4. Push (and set upstream)
git push -u origin main
```

---

## SSH vs HTTPS

### HTTPS
```bash
git clone https://github.com/you/repo.git
# You'll need to authenticate each push (or use credential manager)
```

### SSH (recommended for regular use)
```bash
git clone git@github.com:you/repo.git
# After one-time SSH key setup, no password prompts
```

**Setting up SSH keys:**
```bash
# 1. Generate a key pair
ssh-keygen -t ed25519 -C "your@email.com"

# 2. Start the SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Copy your PUBLIC key
cat ~/.ssh/id_ed25519.pub
# Paste this into GitHub → Settings → SSH Keys

# 4. Test connection
ssh -T git@github.com
# "Hi username! You've successfully authenticated."
```

---

## Tracking Branches

When you clone or push with `-u`, Git creates a **tracking relationship** between your local branch and its remote counterpart.

```bash
# See tracking relationships for all branches
git branch -vv

# Output:
# * main        3a4b5c6 [origin/main] Add authentication
#   feature-x   1a2b3c4 [origin/feature-x] Start feature
```

The `[origin/main]` shows what the branch tracks.

---

## Renaming and Removing Remotes

```bash
# Rename a remote
git remote rename origin upstream

# Change a remote's URL
git remote set-url origin https://github.com/you/new-name.git

# Remove a remote
git remote remove old-remote
```

---

## Multiple Remotes

You can have multiple remotes. Common when you've forked a project:

```bash
git remote add upstream https://github.com/original/repo.git

# See both
git remote -v
# origin    https://github.com/you/repo.git (fetch)
# upstream  https://github.com/original/repo.git (fetch)

# Pull updates from the original project
git fetch upstream
git merge upstream/main
```

---

## Common Mistakes

> [!bug] Push rejected: "non-fast-forward"
> Someone else pushed to the remote since your last pull. You need to fetch and merge/rebase first:
> ```bash
> git pull --rebase
> git push
> ```

> [!bug] Accidentally pushed to wrong branch
> If the push just happened and it's a personal repo, you can force-push a correction. On shared repos, you need to `git revert` the unwanted commits and push that.

> [!bug] "Repository not found" when pushing
> Your remote URL might be wrong, or you don't have permission. Check: `git remote -v`.

> [!bug] Typing your password every time (HTTPS)
> Enable credential storage:
> ```bash
> git config --global credential.helper store   # Linux (plain text, not ideal)
> git config --global credential.helper osxkeychain  # macOS
> ```
> Better: switch to SSH keys.

---

## Exercises

1. Create a new public repo on GitHub. Connect your local repo and push your commits.
2. Edit a file on GitHub's web UI and `git pull` the change to your local machine.
3. Set up SSH key authentication and push using SSH.
4. Clone any repo, add a second remote, and fetch from both.

---

## Related Notes

- [[03 Your First Repository]] — Creating local repos
- [[09 Branching Basics]] — Working with remote branches
- [[10 Team Workflows]] — Collaboration patterns (forks, PRs)

---

## Official References

- [`git remote` docs](https://git-scm.com/docs/git-remote)
- [`git push` docs](https://git-scm.com/docs/git-push)
- [`git pull` docs](https://git-scm.com/docs/git-pull)
- [`git fetch` docs](https://git-scm.com/docs/git-fetch)
- [Pro Git: Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

---

#git/beginner #git/remote #git/push #git/pull #git/github

---
title: Troubleshooting Hub
tags: [git, troubleshooting, help, errors]
type: hub
---

# 🔧 Troubleshooting Hub

> Find your problem, get the fix. Every scenario links to the relevant deep-dive note.

---

## "I made a mistake — how do I undo it?"

Use this decision tree:

```
Where is the mistake?
│
├── Working tree (not staged yet)
│     → git restore <file>
│     → See [[07 Undoing Things]]
│
├── Staging area (staged but not committed)
│     → git restore --staged <file>
│     → See [[07 Undoing Things]]
│
├── Last commit (not pushed yet)
│     → git commit --amend         (fix message or add files)
│     → git reset --soft HEAD~1    (undo, keep staged)
│     → git reset HEAD~1           (undo, keep unstaged)
│     → git reset --hard HEAD~1    (undo, discard ⚠️)
│     → See [[07 Undoing Things]]
│
├── Several commits ago (not pushed)
│     → git rebase -i HEAD~N
│     → See [[02 Interactive Rebase]]
│
├── Already pushed to shared branch
│     → git revert <hash>   (safe — creates new undo commit)
│     → See [[07 Undoing Things]]
│
└── I "lost" a commit (hard reset, deleted branch, bad rebase)
      → git reflog
      → See [[06 Reflog]]
```

---

## Merge & Rebase Problems

### "I have merge conflicts"

```bash
# See conflicted files
git status

# Open merge tool
git mergetool

# For each file: resolve manually, then
git add <resolved-file>

# Complete the merge
git commit

# OR bail out entirely
git merge --abort
```

→ [[10 Merging]]

### "Rebase stopped with conflicts"

```bash
# Resolve conflict in the file, then:
git add <file>
git rebase --continue

# Skip this commit (use carefully)
git rebase --skip

# Bail out entirely
git rebase --abort
```

→ [[01 Rebase]]

### "I rebased and now my history is gone"

```bash
git reflog
# Find the pre-rebase HEAD entry: "rebase: checkout" line
git reset --hard HEAD@{n}    # n = the step before rebase started
```

→ [[06 Reflog]]

---

## Push & Pull Problems

### "Push rejected: non-fast-forward"

Someone pushed to the remote before you. You must integrate their changes first:

```bash
git pull --rebase    # recommended: rebase your commits on top
git push
```

→ [[08 Working with Remotes]]

### "I pushed to the wrong branch"

```bash
# If it was just pushed and no one has pulled it:
git push origin --delete wrong-branch   # delete the bad push
git push -u origin correct-branch       # push to correct branch

# If others may have pulled:
# Use git revert on the wrong branch to undo, then push
```

### "I can't push — permission denied"

```bash
# Check remote URL
git remote -v

# If HTTPS, re-authenticate or switch to SSH
git remote set-url origin git@github.com:you/repo.git

# Test SSH connection
ssh -T git@github.com
```

→ [[08 Working with Remotes]]

### "git pull has conflicts"

```bash
# Option A: merge the conflict manually
git pull
# resolve conflicts → git add → git commit

# Option B: use rebase instead
git pull --rebase
# resolve conflicts per-commit → git add → git rebase --continue
```

---

## Branch Problems

### "I committed to main instead of a feature branch"

```bash
# Create the branch at current position
git branch feature/my-work

# Move main back to where it was before your commits
git reset --hard origin/main

# Switch to your branch
git switch feature/my-work
# Your commits are safe on the branch
```

### "I accidentally deleted a branch"

```bash
git reflog
# Find the last commit hash of that branch
# e.g.: "abc1234 HEAD@{3}: commit: Last commit on deleted-branch"

git branch recovered-name abc1234
```

→ [[06 Reflog]]

### "Detached HEAD — what do I do?"

```bash
# You're not on any branch. To save your work:
git switch -c my-new-branch    # creates branch at current position

# To just go back to main (discards detached commits):
git switch main
```

→ [[09 Branching Basics]]

---

## Commit & History Problems

### "I committed a secret / API key"

1. **Rotate the secret immediately** — assume it's compromised
2. Remove from tracking and add to `.gitignore`
3. Scrub from history (before pushing):
   ```bash
   git rebase -i HEAD~N    # drop or edit the offending commit
   ```
4. If already pushed, use `git filter-repo`:
   ```bash
   pip install git-filter-repo
   git filter-repo --path secrets.env --invert-paths
   git push --force --all
   ```
5. Notify your team — force push affects everyone

→ [[11 gitignore]], [[02 Interactive Rebase]]

### "I committed a huge file / node_modules"

```bash
# If not pushed yet:
git rm -r --cached node_modules/
echo "node_modules/" >> .gitignore
git commit -m "Remove node_modules from tracking"

# If pushed: use git-filter-repo to scrub history
git filter-repo --path node_modules --invert-paths
```

### "My git log shows a merge commit I don't want"

```bash
# If local only (not pushed):
git reset --hard HEAD~1   # removes the merge commit
```

---

## Submodule Problems

### "Submodule directory is empty after clone"

```bash
git submodule update --init --recursive
```

### "Submodule is in detached HEAD"

```bash
cd my-submodule
git switch main    # switch to a branch inside the submodule
```

→ [[08 Submodules]]

---

## Performance Problems

### "git status is slow on large repo"

```bash
# Enable fsmonitor for faster status
git config core.fsmonitor true
git config core.untrackedcache true

# Or use sparse checkout to limit working tree
git sparse-checkout init --cone
git sparse-checkout set path/you/need
```

→ [[06 Sparse Checkout and Partial Clone]]

### "Repository is very large"

```bash
# Run garbage collection
git gc --aggressive

# Check what's taking space
git count-objects -vH

# Find large files
git rev-list --objects --all | \
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | \
  awk '/^blob/ {print substr($0,6)}' | sort -k2nr | head -20
```

→ [[04 Packfiles and GC]]

---

## Reference

→ [[⚡ Command Reference]] — All commands
→ [[06 Reflog]] — Recovery tool
→ [[07 Undoing Things]] — Undo operations
→ [[🏠 Home]] — Back to home

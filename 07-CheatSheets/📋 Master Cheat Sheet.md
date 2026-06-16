---
title: Master Cheat Sheet
tags: [git, cheatsheet, reference]
type: cheatsheet
---

# 📋 Git Master Cheat Sheet

> Print this. Tape it to your monitor. You're welcome.

---

## ⚙️ Setup (one-time)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --global pull.rebase true
git config --global alias.lg "log --oneline --graph --all --decorate"
```

---

## 📁 Start a Repo

```bash
git init                          # new repo here
git clone <url>                   # copy from remote
git clone --recurse-submodules <url>  # with submodules
```

---

## 📸 Daily Commit Cycle

```bash
git status                        # what's happening?
git diff                          # what exactly changed?
git add <file>                    # stage file
git add .                         # stage everything
git add -p                        # stage interactively (hunks)
git diff --staged                 # review what you're about to commit
git commit -m "type: message"     # commit
git commit -am "message"          # stage tracked + commit
git commit --amend                # fix last commit
```

---

## 🌿 Branches

```bash
git branch                        # list local
git branch -a                     # list all (+ remote)
git switch -c <name>              # create + switch
git switch <name>                 # switch
git branch -d <name>              # delete (merged only)
git branch -D <name>              # force delete
git branch -m old new             # rename
```

---

## 🔀 Merge & Rebase

```bash
git merge <branch>                # merge into current
git merge --abort                 # bail out of conflict
git rebase main                   # rebase current onto main
git rebase -i HEAD~3              # interactive rebase (last 3)
git rebase --continue             # after resolving conflict
git rebase --abort                # bail out
```

---

## 🌐 Remote

```bash
git remote -v                     # list remotes
git remote add origin <url>       # add remote
git fetch                         # download, don't merge
git pull                          # fetch + merge
git pull --rebase                 # fetch + rebase
git push                          # push current branch
git push -u origin <branch>       # push + set upstream
git push --force-with-lease       # force push (safer)
```

---

## ↩️ Undo

```bash
git restore <file>                # discard working tree ⚠️
git restore --staged <file>       # unstage
git reset --soft HEAD~1           # undo commit, keep staged
git reset HEAD~1                  # undo commit, keep unstaged
git reset --hard HEAD~1           # undo commit + changes ⚠️
git revert HEAD                   # safe undo (makes new commit)
git reflog                        # find "lost" commits
```

---

## 📦 Stash

```bash
git stash push -m "desc"          # save WIP
git stash push -u -m "desc"       # save + untracked files
git stash list                    # see stashes
git stash pop                     # restore + remove
git stash apply stash@{n}         # restore, keep it
git stash drop stash@{n}          # delete one
git stash clear                   # delete all ⚠️
```

---

## 🔍 History & Inspect

```bash
git log --oneline                 # compact log
git log --oneline --graph --all   # visual graph
git log --grep="keyword"          # search messages
git log -S "function"             # search code changes
git log --follow -p <file>        # file's full history
git diff <branch1>..<branch2>     # compare branches
git show <commit>                 # full commit details
git blame <file>                  # who wrote each line
```

---

## 🏷️ Tags

```bash
git tag -a v1.0.0 -m "Release"   # annotated tag
git push origin v1.0.0            # push a tag
git push origin --tags            # push all tags
git tag -d v1.0.0                 # delete local tag
git push origin --delete v1.0.0   # delete remote tag
```

---

## 🍒 Cherry-pick

```bash
git cherry-pick <hash>            # apply one commit here
git cherry-pick <h1> <h2>         # apply multiple
git cherry-pick --abort           # bail out
```

---

## 🔎 Bisect (find the bad commit)

```bash
git bisect start
git bisect bad                    # current is broken
git bisect good <ref>             # this was working
# test, then: git bisect good OR git bisect bad
git bisect run <test-script>      # automated
git bisect reset                  # done
```

---

## Commit Message Format (Conventional Commits)

```
feat(scope): add new feature
fix(scope): fix bug
docs: update README
refactor: improve code structure
test: add unit tests
chore: update dependencies
feat!: breaking change
```

---

## Symbols Key

| Symbol | Meaning |
| ------ | ------- |
| ⚠️ | Destructive — cannot be easily undone |
| `HEAD` | Current position (tip of current branch) |
| `HEAD~1` | One commit before HEAD |
| `HEAD~3` | Three commits before HEAD |
| `origin` | Default remote name |
| `main` | Default branch name |

---

→ Full details: [[⚡ Command Reference]]
→ Fix problems: [[🔧 Troubleshooting Hub]]
→ Home: [[🏠 Home]]

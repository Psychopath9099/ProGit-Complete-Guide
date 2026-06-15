---
title: Command Reference
tags: [git, command, reference, cheatsheet]
type: reference
---

# ⚡ Command Reference

> Quick lookup for every Git command covered in this vault. For full details, click the linked note.

---

## Setup & Config

```bash
git config --global user.name "Name"          # set identity
git config --global user.email "e@mail.com"   # set email
git config --global core.editor "code --wait" # set editor
git config --global init.defaultBranch main   # default branch name
git config --list --show-origin               # show all settings + sources
git config user.name                          # read a value
```

→ [[02 Installing and Configuring Git]]

---

## Repository

```bash
git init                              # create repo in current folder
git init my-project                   # create repo in new folder
git clone <url>                       # clone a remote repo
git clone <url> <dirname>             # clone into specific folder
git clone --recurse-submodules <url>  # clone with submodules
```

→ [[03 Your First Repository]]

---

## Status & Inspection

```bash
git status                # what's changed, staged, untracked
git status -s             # short format
git diff                  # unstaged changes
git diff --staged         # staged changes (vs last commit)
git diff HEAD             # all changes since last commit
git diff <branch1>..<branch2>  # diff between branches
git show <commit>         # show a commit's changes
git show HEAD             # show most recent commit
```

→ [[05 Staging and Committing]], [[06 Viewing History]]

---

## Staging

```bash
git add <file>            # stage a file
git add .                 # stage all changes in current dir
git add -A                # stage all (new + modified + deleted)
git add -p                # interactive: choose hunks to stage
git restore --staged <file>   # unstage a file (keep changes)
git restore --staged .        # unstage everything
```

→ [[05 Staging and Committing]]

---

## Committing

```bash
git commit -m "message"           # commit with message
git commit                        # open editor for message
git commit -am "message"          # stage tracked + commit
git commit --amend                # edit last commit
git commit --amend -m "new msg"   # edit last commit message
git commit --amend --no-edit      # add to last commit, keep msg
```

→ [[05 Staging and Committing]]

---

## History

```bash
git log                                # full log
git log --oneline                      # one line per commit
git log --oneline --graph --all        # visual branch graph
git log -5                             # last 5 commits
git log --since="2 weeks ago"          # filter by date
git log --author="Name"               # filter by author
git log --grep="keyword"              # search messages
git log -S "function_name"            # search for added/removed string
git log --follow -p <file>            # history of a file
git log --stat                        # with file change summary
git blame <file>                      # who wrote each line
git blame -L 10,20 <file>             # specific lines
```

→ [[06 Viewing History]]

---

## Undoing

```bash
git restore <file>                  # discard working tree changes ⚠️
git restore .                       # discard ALL working tree changes ⚠️
git restore --staged <file>         # unstage
git commit --amend                  # fix last commit
git reset --soft HEAD~1             # undo last commit, keep staged
git reset HEAD~1                    # undo last commit, keep unstaged
git reset --hard HEAD~1             # undo last commit, discard changes ⚠️
git revert HEAD                     # safe undo: creates new commit
git revert <hash>                   # revert specific commit
git reflog                          # find anything you "lost"
```

→ [[07 Undoing Things]], [[06 Reflog]]

---

## Branches

```bash
git branch                          # list local branches
git branch -a                       # list all (including remote)
git branch -v                       # with last commit
git branch <name>                   # create branch
git switch <name>                   # switch to branch
git switch -c <name>                # create + switch
git switch -c <name> <commit>       # branch from specific commit
git branch -d <name>                # delete merged branch
git branch -D <name>                # force delete
git push origin --delete <branch>   # delete remote branch
git branch -m old-name new-name     # rename branch
```

→ [[09 Branching Basics]]

---

## Merging

```bash
git merge <branch>                  # merge branch into current
git merge --no-ff <branch>          # always create merge commit
git merge --squash <branch>         # squash into one, don't commit
git merge --abort                   # abort in-progress merge
git mergetool                       # open visual merge tool
git checkout --ours <file>          # take our version in conflict
git checkout --theirs <file>        # take their version in conflict
```

→ [[10 Merging]]

---

## Remotes

```bash
git remote -v                       # list remotes
git remote add origin <url>         # add remote
git remote remove <name>            # remove remote
git remote rename <old> <new>       # rename remote
git remote set-url origin <url>     # change URL
git fetch                           # download, no merge
git fetch origin                    # fetch from origin
git pull                            # fetch + merge
git pull --rebase                   # fetch + rebase
git push                            # push current branch
git push -u origin <branch>         # push + set upstream
git push --force-with-lease         # force push (safer)
git push origin --tags              # push all tags
git push origin --delete <branch>   # delete remote branch
```

→ [[08 Working with Remotes]]

---

## Rebase

```bash
git rebase main                     # rebase current onto main
git rebase --onto main base feature # rebase feature onto main, excluding base
git rebase -i HEAD~3                # interactive rebase last 3 commits
git rebase -i main                  # interactive rebase since branching from main
git rebase --continue               # continue after resolving conflict
git rebase --abort                  # abort rebase
git rebase --skip                   # skip current conflicting commit
git pull --rebase                   # fetch + rebase instead of merge
```

→ [[01 Rebase]], [[02 Interactive Rebase]]

---

## Stash

```bash
git stash                           # stash current changes
git stash push -m "description"     # stash with label
git stash push -u -m "desc"         # include untracked files
git stash list                      # see all stashes
git stash pop                       # apply latest + remove
git stash apply stash@{2}           # apply specific, keep it
git stash drop stash@{0}            # delete a stash
git stash clear                     # delete all stashes
git stash show -p stash@{0}         # see stash diff
git stash branch <name> stash@{0}   # branch from stash
```

→ [[04 Stash]]

---

## Tags

```bash
git tag                             # list tags
git tag -a v1.0.0 -m "message"      # create annotated tag
git tag v1.0.0                      # create lightweight tag
git show v1.0.0                     # show tag details
git push origin v1.0.0              # push specific tag
git push origin --tags              # push all tags
git tag -d v1.0.0                   # delete local tag
git push origin --delete v1.0.0     # delete remote tag
git checkout v1.0.0                 # checkout at tag (detached HEAD)
git switch -c <branch> v1.0.0       # branch from tag
```

→ [[05 Tags]]

---

## Cherry-pick

```bash
git cherry-pick <hash>              # apply a commit to current branch
git cherry-pick <hash1> <hash2>     # apply multiple commits
git cherry-pick abc..def            # apply a range
git cherry-pick --no-commit <hash>  # apply without committing
git cherry-pick --abort             # abort
git cherry-pick --continue          # continue after conflict
```

→ [[03 Cherry-pick]]

---

## Submodules

```bash
git submodule add <url> <path>      # add a submodule
git submodule update --init --recursive  # init + fetch all
git submodule update --remote       # update to latest
git submodule foreach git pull      # pull in all submodules
git submodule status                # status of all submodules
git submodule deinit <path>         # remove submodule
```

→ [[08 Submodules]]

---

## Debugging

```bash
git bisect start                    # start binary search
git bisect bad                      # mark current as bad
git bisect good <ref>               # mark a good commit
git bisect run <script>             # automated bisect
git bisect reset                    # end bisect
git reflog                          # full history of HEAD movement
git reflog --date=relative          # with timestamps
git fsck                            # verify object database
git log -S "string"                 # find when string was added/removed
git blame <file>                    # who changed each line
```

→ [[07 Bisect]], [[06 Reflog]]

---

## Advanced / Plumbing

```bash
git cat-file -p <hash>              # print object contents
git cat-file -t <hash>              # object type
git hash-object <file>              # get SHA-1 of file
git ls-tree HEAD                    # list tree
git ls-files                        # list index
git rev-parse HEAD                  # resolve ref to hash
git for-each-ref refs/heads/        # list all branch refs
git gc                              # garbage collect
git count-objects -vH               # repo size info
```

→ [[05 Plumbing Commands]], [[01 Git Object Model]]

---

→ Back to [[🏠 Home]]
→ Quick summary: [[📋 Master Cheat Sheet]]

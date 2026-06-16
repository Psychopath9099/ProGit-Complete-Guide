---
title: Workflows Hub
tags: [git, workflow, reference]
type: hub
---

# 🔀 Workflows Hub

---

## Solo Developer Workflow

```bash
# Daily loop:
git pull                          # sync from remote
git switch -c feature/thing       # new branch
# ... work, work, work ...
git add -p                        # stage carefully
git commit -m "feat: add thing"   # commit
git push -u origin feature/thing  # push
# merge to main (on GitHub or locally)
git switch main && git merge feature/thing
git push
git branch -d feature/thing
```

---

## GitHub Flow (Teams)

```
main (always deployable)
 │
 ├── feature/login     → PR → review → merge → delete
 ├── fix/payment-bug   → PR → review → merge → delete
 └── docs/readme       → PR → review → merge → delete
```

```bash
git switch main && git pull
git switch -c feature/user-auth
# ... commits ...
git push -u origin feature/user-auth
# Open Pull Request → get review → merge on GitHub
git switch main && git pull
git branch -d feature/user-auth
```

→ [[10 Team Workflows]]

---

## Keeping Feature Branch Updated

```bash
# Option A: Rebase (clean, linear history)
git switch feature/my-work
git fetch origin
git rebase origin/main
git push --force-with-lease

# Option B: Merge (preserves context)
git switch feature/my-work
git merge main
git push
```

---

## Release Workflow

```bash
# 1. Feature freeze on main/develop
# 2. Create release branch
git switch -c release/v1.2.0 main

# 3. Bump version, update changelog
git commit -am "chore: bump version to 1.2.0"

# 4. Tag and merge to main
git switch main
git merge --no-ff release/v1.2.0
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin main --follow-tags

# 5. Clean up
git branch -d release/v1.2.0
```

---

## Emergency Hotfix Workflow

```bash
# Branch from the tagged production release
git switch -c hotfix/critical-bug v1.2.0

# Fix it
git commit -am "fix: resolve critical payment null pointer"

# Merge to main
git switch main
git merge --no-ff hotfix/critical-bug
git tag -a v1.2.1 -m "Hotfix v1.2.1"
git push origin main --follow-tags

# Merge to develop too (if using Gitflow)
git switch develop
git merge --no-ff hotfix/critical-bug

git branch -d hotfix/critical-bug
```

---

## Fork & PR Workflow (Open Source)

```bash
# 1. Fork on GitHub

# 2. Clone your fork
git clone git@github.com:YOU/repo.git
git remote add upstream git@github.com:ORIGINAL/repo.git

# 3. Sync before starting work
git fetch upstream
git switch main
git merge upstream/main

# 4. Make a branch
git switch -c fix/my-contribution

# 5. Commit and push to YOUR fork
git commit -am "fix: correct typo in docs"
git push origin fix/my-contribution

# 6. Open PR from your fork to upstream on GitHub

# 7. After PR is merged, sync again
git fetch upstream
git switch main && git merge upstream/main
git push origin main
```

---

→ [[10 Team Workflows]] — Full workflow notes
→ [[09 Git Hooks]] — Automate workflow enforcement
→ [[🏠 Home]] — Home

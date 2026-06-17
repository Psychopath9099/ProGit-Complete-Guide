---
title: Tags
tags: [git, intermediate, tags, releases]
type: command-note
difficulty: intermediate
official-doc: https://git-scm.com/docs/git-tag
related:
  - "[[06 Viewing History]]"
  - "[[10 Team Workflows]]"
---

# Tags

## Overview

A **tag** is a permanent, human-readable pointer to a specific commit — usually used to mark release versions (`v1.0.0`, `v2.3.1`). Unlike branches, tags don't move as you make new commits. They're bookmarks in history.

---

## Two Types of Tags

### Lightweight Tags
A simple pointer to a commit. Like a branch that never moves. No extra metadata.

```bash
git tag v1.0.0
git tag v1.0.0 abc1234   # tag a specific commit
```

### Annotated Tags (recommended)
A full object in Git's database. Stores: tagger name, email, date, message, and can be GPG-signed.

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git tag -a v1.0.0 abc1234 -m "Release version 1.0.0"
```

> [!tip] Always use annotated tags for releases
> Annotated tags are richer, searchable, and can be signed. Lightweight tags are only for personal temporary markers.

---

## Listing Tags

```bash
# All tags
git tag

# Filter by pattern
git tag -l "v1.*"
# v1.0.0
# v1.1.0
# v1.2.0

# With messages (annotated only)
git tag -n
# v1.0.0  Release version 1.0.0
# v1.1.0  Release version 1.1.0
```

---

## Viewing a Tag

```bash
git show v1.0.0

# Output includes:
# tag v1.0.0
# Tagger: Jane Dev <jane@example.com>
# Date:   Mon Jan 13 14:32:01 2025 +0530
#
# Release version 1.0.0
#
# commit abc1234...
# Author: Jane Dev...
# ...
```

---

## Pushing Tags to Remote

Tags are NOT pushed automatically with `git push`. You must push them explicitly:

```bash
# Push a specific tag
git push origin v1.0.0

# Push ALL local tags not on remote
git push origin --tags

# Push all annotated tags only (not lightweight)
git push origin --follow-tags
```

> [!tip] Use `--follow-tags` in CI/CD
> `git push --follow-tags` pushes your commits AND any annotated tags that point to them. Good for automated release pipelines.

---

## Deleting Tags

```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
# or
git push origin :refs/tags/v1.0.0
```

---

## Checking Out a Tag

```bash
# Visit the state of the code at a tag (detached HEAD)
git checkout v1.0.0
# "HEAD is now at abc1234... Release version 1.0.0"

# Create a branch from a tag (to make changes)
git switch -c hotfix/v1.0.1 v1.0.0
```

---

## Semantic Versioning Convention

Most projects use [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`

```
v1.0.0   — first stable release
v1.0.1   — patch: backward-compatible bug fix
v1.1.0   — minor: backward-compatible new feature
v2.0.0   — major: breaking changes
v2.0.0-beta.1  — pre-release
v2.0.0-rc.1    — release candidate
```

---

## Tagging Workflow (Release Process)

```bash
# 1. Make sure you're on the right commit
git switch main
git pull

# 2. Create annotated tag
git tag -a v1.2.0 -m "Release v1.2.0: Add payment integration"

# 3. Push the tag
git push origin v1.2.0

# 4. On GitHub, this creates a release automatically
#    Go to Releases → Draft a new release → select your tag
```

---

## Comparing Releases

```bash
# What changed between v1.0.0 and v1.1.0?
git log v1.0.0..v1.1.0 --oneline

# Diff between two releases
git diff v1.0.0 v1.1.0

# Diff for a specific file between releases
git diff v1.0.0 v1.1.0 -- src/payment.js
```

---

## Quick Reference

| Task | Command |
| ---- | ------- |
| Create annotated tag | `git tag -a v1.0.0 -m "message"` |
| Create lightweight tag | `git tag v1.0.0` |
| List all tags | `git tag` |
| Show tag details | `git show v1.0.0` |
| Push a tag | `git push origin v1.0.0` |
| Push all tags | `git push origin --tags` |
| Delete local tag | `git tag -d v1.0.0` |
| Delete remote tag | `git push origin --delete v1.0.0` |
| Checkout at tag | `git checkout v1.0.0` |
| Branch from tag | `git switch -c branch-name v1.0.0` |

---

## Related Notes

- [[06 Viewing History]] — Navigate history using tags
- [[10 Team Workflows]] — Tags in release workflows

---

## Official References

- [`git tag` docs](https://git-scm.com/docs/git-tag)
- [Pro Git: Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

---

#git/intermediate #git/tags #git/releases

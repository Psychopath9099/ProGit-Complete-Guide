---
title: Advanced MOC — Git Internals & Power Tools
type: moc
tags: [git, advanced, moc]
difficulty: advanced
---

# 🔴 Advanced Map of Content

> **Goal:** Understand Git at the object level. Know what every command does under the hood. Debug anything.

---

## All Advanced Notes

| # | Note | Key Commands |
|---|---|---|
| 1 | [[01 Git Object Model]] | `git cat-file`, `git hash-object` |
| 2 | [[02 The Git Index]] | `git ls-files`, `git update-index` |
| 3 | [[03 Refs and HEAD]] | `git symbolic-ref`, `git update-ref` |
| 4 | [[04 Packfiles and GC]] | `git gc`, `git verify-pack` |
| 5 | [[05 Plumbing Commands]] | `git rev-parse`, `git ls-tree` |
| 6 | [[06 Sparse Checkout and Partial Clone]] | `git sparse-checkout` |

---

## Key Insight

> [!important] Everything is an Object
> Git's database stores four types of objects: **blob** (file content), **tree** (directory), **commit** (snapshot + metadata), **tag** (annotated pointer).
> Every object is identified by its SHA-1 hash. This is why Git has integrity — you can't change history without the hashes changing.

→ [[🏠 Home|← Back to Home]]

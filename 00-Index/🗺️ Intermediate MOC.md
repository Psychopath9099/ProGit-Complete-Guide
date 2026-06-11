---
title: Intermediate MOC — Professional Git
type: moc
tags: [git, intermediate, moc]
difficulty: intermediate
---

# 🟡 Intermediate Map of Content

> **Goal:** Use Git confidently in professional settings — team workflows, clean history, advanced branching, and debugging.

---

## Prerequisites

Before starting here, you should be comfortable with:
- ✅ Creating commits and branches
- ✅ Pushing/pulling from remotes
- ✅ Basic merging

---

## Learning Path

```
[1]  Rebase — the alternative to merge
      ↓
[2]  Interactive Rebase — rewriting history
      ↓
[3]  Cherry-pick — applying individual commits
      ↓
[4]  Stash — saving work in progress
      ↓
[5]  Tags — marking releases
      ↓
[6]  Reflog — your safety net
      ↓
[7]  Bisect — finding the commit that broke things
      ↓
[8]  Submodules — managing dependencies
      ↓
[9]  Git Hooks — automating your workflow
      ↓
[10] Team Workflows — GitHub Flow, Gitflow
      ↓
    ✅ Ready for Advanced
```

---

## All Intermediate Notes

1. [[01 Rebase]]
2. [[02 Interactive Rebase]]
3. [[03 Cherry-pick]]
4. [[04 Stash]]
5. [[05 Tags]]
6. [[06 Reflog]]
7. [[07 Bisect]]
8. [[08 Submodules]]
9. [[09 Git Hooks]]
10. [[10 Team Workflows]]

---

## Key Concepts

> [!important] Rebase vs Merge
> - **Merge** preserves history exactly as it happened
> - **Rebase** rewrites history for a cleaner, linear story
>
> Rule of thumb: merge for public branches, rebase for local cleanup.

> [!warning] Golden Rule of Rebasing
> **Never rebase commits that have been pushed to a shared branch.**
> Rewriting shared history breaks everyone else's work.

> [!tip] The Reflog is Your Safety Net
> Even after a "destructive" operation (hard reset, rebase, amend), your commits are not gone for 90 days. `git reflog` shows you everything Git has touched.

---

## After Completing This Section

→ Move to [[🗺️ Advanced MOC]]
→ Try [[Project 2 — Feature Branch Workflow]]
→ Try [[Project 3 — Team Collaboration Simulation]]

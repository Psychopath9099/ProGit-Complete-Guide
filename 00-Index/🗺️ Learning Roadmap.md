---
title: Learning Roadmap
tags: [git, roadmap, learning]
type: roadmap
---

# 🗺️ Learning Roadmap

A structured, time-estimated path from zero to proficient Git user.

---

## Phase 1 — Beginner (Week 1–2)

**Goal:** Understand what Git does and use it confidently solo.

### Day 1–2: Theory & Setup
- [ ] [[01 What is Git]] — Read fully, understand snapshots vs deltas
- [ ] [[02 Installing and Configuring Git]] — Complete setup, verify with `git config --list`
- [ ] [[03 Your First Repository]] — Do both `init` and `clone` walkthroughs

### Day 3–4: The Core Loop
- [ ] [[04 The Three States]] — **Read twice.** Draw the diagram on paper.
- [ ] [[05 Staging and Committing]] — Practice `add -p` interactive staging
- [ ] **Mini-exercise:** Make 10 commits on a test repo. Each one deliberate and well-messaged.

### Day 5–6: History & Undoing
- [ ] [[06 Viewing History]] — Practice every `git log` variant
- [ ] [[07 Undoing Things]] — Run every scenario in the exercises section
- [ ] **Mini-exercise:** Intentionally make mistakes and undo them all.

### Day 7–8: Remotes
- [ ] [[08 Working with Remotes]] — Push a real repo to GitHub
- [ ] Set up SSH keys (the one-time investment that pays forever)

### Day 9–10: Branching & Merging
- [ ] [[09 Branching Basics]] — Create 5 branches, switch between them
- [ ] [[10 Merging]] — Deliberately create and resolve a merge conflict
- [ ] [[11 gitignore]] — Add `.gitignore` to your real projects

### Day 11–14: Polish & Practice
- [ ] [[12 Git Aliases]] — Set up your `~/.gitconfig` fully
- [ ] [[Project 1 — Personal Portfolio Repo]] — Complete the project
- [ ] Review [[📋 Master Cheat Sheet]] — Internalize the daily commands

**Milestone:** You can manage your own projects with Git without thinking hard about it.

---

## Phase 2 — Intermediate (Week 3–5)

**Goal:** Use Git professionally in team contexts. Rewrite history cleanly. Recover from anything.

### Week 3: Rewriting History
- [ ] [[01 Rebase]] — Rebase a feature branch onto main
- [ ] [[02 Interactive Rebase]] — Squash a messy 5-commit history into 2 clean commits
- [ ] [[03 Cherry-pick]] — Cherry-pick a fix from one branch to another
- [ ] **Practice:** Every feature branch you create, rebase it before merging

### Week 4: Safety Nets & Power Tools
- [ ] [[06 Reflog]] — Deliberately "destroy" things and recover them all
- [ ] [[04 Stash]] — Use stash in a real context (urgent interruption)
- [ ] [[05 Tags]] — Tag a release in a project
- [ ] [[07 Bisect]] — Use `bisect run` with a test script

### Week 5: Team & Automation
- [ ] [[08 Submodules]] — Add and update a real submodule
- [ ] [[09 Git Hooks]] — Write a `pre-commit` hook for one of your projects
- [ ] [[10 Team Workflows]] — Read all three workflows; pick one for your team
- [ ] [[Project 2 — Feature Branch Workflow]] — Complete all 5 exercises
- [ ] [[Project 3 — Team Collaboration Simulation]] — Run the full simulation

**Milestone:** You can handle any typical team Git scenario. You don't panic when things go wrong.

---

## Phase 3 — Advanced (Ongoing)

**Goal:** Understand Git's internals. Debug anything. Extend Git.

- [ ] [[01 Git Object Model]] — Inspect real objects with `git cat-file`
- [ ] [[02 The Git Index]] — Understand the staging area at the binary level
- [ ] [[03 Refs and HEAD]] — Read every ref file in `.git/`
- [ ] [[04 Packfiles and GC]] — Run `git gc` and inspect the result
- [ ] [[05 Plumbing Commands]] — Manually create a commit using only plumbing
- [ ] [[06 Sparse Checkout and Partial Clone]] — Try on a large public repo

**Milestone:** You can explain what Git does at every step. You can write scripts that manipulate Git directly.

---

## Daily Practice Habits

Once you're past Week 1, make these automatic:

1. **Always branch** — never commit directly to `main`
2. **Commit small** — one logical change per commit
3. **Write real messages** — subject line explains the *why*
4. **Check before pushing** — `git log --oneline -5` and `git diff --staged`
5. **Pull before you push** — `git pull --rebase` keeps history clean

---

## Recommended Reading Order (by Note)

```
01 What is Git
02 Installing and Configuring Git
03 Your First Repository
04 The Three States          ← re-read after 1 week
05 Staging and Committing
06 Viewing History
07 Undoing Things
08 Working with Remotes
09 Branching Basics
10 Merging
11 gitignore
12 Git Aliases
── Project 1 ──
01 Rebase
02 Interactive Rebase
03 Cherry-pick
04 Stash
05 Tags
06 Reflog
07 Bisect
08 Submodules
09 Git Hooks
10 Team Workflows
── Projects 2 & 3 ──
01 Git Object Model          ← advanced starts here
02 The Git Index
03 Refs and HEAD
04 Packfiles and GC
05 Plumbing Commands
06 Sparse Checkout
```

---

→ [[🏠 Home]]
→ [[🗺️ Beginner MOC]]
→ [[📖 Glossary]]

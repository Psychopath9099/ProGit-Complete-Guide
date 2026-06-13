---
title: What is Git?
tags: [git, beginner, concepts]
type: concept-note
difficulty: beginner
official-doc: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F
related:
  - "[[02 Installing and Configuring Git]]"
  - "[[03 Your First Repository]]"
  - "[[04 The Three States]]"
---

# What is Git?

## Overview

Git is a **distributed version control system (DVCS)** — software that tracks changes to your files over time, lets you revert to previous states, and enables multiple people to collaborate on the same project without overwriting each other's work.

Think of it as an **undo history on steroids** for your entire project.

---

## Why It Matters

Without Git, you've probably done something like this:

```
project/
├── my-app-final.js
├── my-app-final-v2.js
├── my-app-ACTUAL-final.js
└── my-app-ACTUAL-final-DO-NOT-DELETE.js
```

Git replaces that chaos with a clean, searchable, reversible history. You can always get back to any version of any file, at any point in time.

---

## The Evolution of Version Control

### 1. Local VCS (old way)
You manually copy files into timestamped folders. Extremely error-prone — easy to overwrite files in the wrong directory.

### 2. Centralized VCS (CVS, Subversion)
A single server holds all versions. Everyone checks files out from that server.
- ✅ Everyone sees the same history
- ❌ If the server goes down, no one can work
- ❌ If the server's hard drive dies, everything is lost

### 3. Distributed VCS — Git (modern)
Every developer has a **complete copy** of the entire repository, including its full history, on their local machine.

```
Developer A           Developer B           Server
[full repo copy]  ←→  [full repo copy]  ←→  [full repo copy]
```

- ✅ Works offline
- ✅ No single point of failure
- ✅ Incredibly fast (most operations are local)

---

## How Git Thinks Differently

> [!important] Snapshots, Not Differences
> Most version control systems store a *list of changes* to each file (delta-based).
>
> **Git stores snapshots.** Every commit is a complete picture of what every file looks like at that moment. If a file hasn't changed, Git stores a link to the previous identical version — it doesn't duplicate data.

**Delta-based (CVS, SVN):**
```
File A: [base] → [change1] → [change2] → [change3]
```

**Git (snapshot-based):**
```
Commit 1: {file_a_v1, file_b_v1, file_c_v1}
Commit 2: {file_a_v1, file_b_v2, file_c_v1}  ← only b changed; a and c are links
Commit 3: {file_a_v2, file_b_v2, file_c_v1}
```

---

## Key Properties of Git

### Nearly Every Operation is Local
History browsing, diffing, committing — it all happens on your disk. No network required.

### Git Has Integrity
Every piece of data is checksummed with a **SHA-1 hash** before storage. You literally cannot change a file without Git knowing about it. A hash looks like this:

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

You'll see these everywhere. Git stores objects by their hash, not by filename.

### Git Generally Only Adds Data
Almost every operation in Git *adds* data to the database. It's very hard to lose committed work. Experiment freely.

---

## A Brief History

- **2002:** Linux kernel project starts using BitKeeper, a proprietary DVCS.
- **2005:** Relationship with BitKeeper breaks down; free access is revoked.
- **2005:** Linus Torvalds (creator of Linux) builds Git in a matter of weeks with goals:
  - Speed
  - Simple design
  - Strong support for non-linear development (thousands of parallel branches)
  - Fully distributed
  - Able to handle large projects efficiently

Git has been the dominant version control system since around 2010.

---

## Vocabulary You'll Encounter

| Term | Plain English |
|---|---|
| **Repository (repo)** | The folder + hidden `.git/` database that stores your project's history |
| **Commit** | A saved snapshot of your project |
| **Branch** | A parallel line of development |
| **Remote** | A copy of the repo on another machine (e.g., GitHub) |
| **Clone** | Copying a remote repo to your local machine |
| **Push** | Sending your commits to a remote |
| **Pull** | Fetching and merging commits from a remote |

---

## Common Misconceptions

> [!warning] "Git = GitHub"
> Git is the tool. GitHub is a website that hosts Git repositories. They are separate things.
> You can use Git entirely without GitHub (locally, or with GitLab, Bitbucket, etc.)

> [!warning] "I only need Git for team projects"
> Even solo, Git is invaluable. It gives you a fearless undo history, lets you experiment on branches, and documents your progress automatically.

---

## Related Notes

- [[02 Installing and Configuring Git]] — Get Git on your machine
- [[04 The Three States]] — The single most important Git concept
- [[03 Your First Repository]] — Start using Git now

---

## Official References

- [Pro Git: Getting Started — About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)
- [Pro Git: What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

---

#git/beginner #git/concepts

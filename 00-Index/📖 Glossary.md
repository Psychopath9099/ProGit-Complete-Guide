---
title: Git Glossary
tags: [git, glossary, reference]
type: reference
---

# 📖 Git Glossary

Alphabetical definitions for every term you'll encounter learning Git.

---

## A

**Annotated Tag**
A tag that is a full Git object — stores tagger name, email, date, message, and can be GPG-signed. Recommended for release markers. See [[05 Tags]].

**Author**
The person who originally wrote a commit. Stored in the commit object as `author` (distinct from `committer`). Set via `git config user.name/email`.

---

## B

**Bare Repository**
A repository with no working tree — just the `.git/` contents directly. Used as a central server repo (`git init --bare`). You can't edit files in it directly.

**Binary Search (Bisect)**
The algorithm `git bisect` uses to find a bad commit in O(log n) steps. See [[07 Bisect]].

**Blob**
A Git object type that stores raw file content (no filename, no path — just bytes). Identified by SHA-1 hash of its content. See [[01 Git Object Model]].

**Branch**
A lightweight movable pointer to a commit. Creating a branch creates a file in `.git/refs/heads/` containing a commit hash. See [[09 Branching Basics]].

---

## C

**Cherry-pick**
Applying the diff introduced by a specific commit onto the current branch, creating a new commit. See [[03 Cherry-pick]].

**Clone**
Creating a local copy of a remote repository, including its full history and a remote called `origin`. See [[03 Your First Repository]].

**Commit**
A Git object that stores: a pointer to a tree (project snapshot), author, committer, timestamp, message, and parent commit hash(es). The fundamental unit of Git history. See [[05 Staging and Committing]].

**Commit Hash / SHA-1**
A 40-character hexadecimal string that uniquely identifies a Git object. Derived from the content of the object. Example: `3b18e512dba79e4c8300dd08aeb37f8e728b8dad`.

**Committer**
The person who applied a commit to the repository. Usually the same as the author, but differs in some workflows (e.g., a maintainer applying someone else's patch).

**Conflict (Merge Conflict)**
When the same part of a file was changed differently on two branches being merged. Git can't auto-resolve it — you must manually choose what to keep. See [[10 Merging]].

---

## D

**DAG (Directed Acyclic Graph)**
The data structure of Git's commit history. Each commit points to its parent(s), forming a graph that flows in one direction (toward the initial commit) with no cycles.

**Default Branch**
The primary branch of a repository. Traditionally called `master`, now typically `main`. Set via `git config init.defaultBranch`.

**Delta Compression**
The technique used in packfiles: instead of storing every file version in full, similar objects are stored as differences (deltas) of each other. Massively reduces repository size. See [[04 Packfiles and GC]].

**Detached HEAD**
A state where `HEAD` points directly to a commit instead of a branch. You can look around and make commits, but those commits won't belong to any branch unless you create one. See [[09 Branching Basics]].

**Distributed Version Control**
A VCS model (like Git) where every developer has a full copy of the entire repository history on their machine, not just the latest snapshot. See [[01 What is Git]].

---

## F

**Fast-forward**
A merge that simply moves a branch pointer forward because there are no divergent commits — no merge commit is created. Only possible if the target branch has no new commits since the source branched. See [[10 Merging]].

**Fetch**
Downloading objects and refs from a remote without merging anything. Safe — never changes your working tree or current branch. See [[08 Working with Remotes]].

**Fork**
A personal copy of someone else's repository on a hosting platform (GitHub/GitLab). Used in open source to propose changes via pull requests without needing write access to the original. See [[10 Team Workflows]].

---

## G

**Git**
A distributed version control system created by Linus Torvalds in 2005 for Linux kernel development. The name is British slang for an unpleasant person — Torvalds named it after himself, jokingly.

**gitignore**
A file (`.gitignore`) listing patterns for files Git should never track. See [[11 gitignore]].

**GitHub / GitLab / Bitbucket**
Web platforms that host Git repositories and add collaboration features (pull requests, issues, CI/CD). Git itself is separate from all of these.

---

## H

**HEAD**
A special ref that points to your current position in the repository — usually a symbolic ref pointing to the current branch. See [[03 Refs and HEAD]].

**Hook**
A script in `.git/hooks/` that Git executes automatically at specific points in the workflow (before commit, before push, etc.). See [[09 Git Hooks]].

---

## I

**Index**
Another name for the staging area. The binary file `.git/index` that holds the next proposed commit. See [[02 The Git Index]].

---

## M

**Merge**
Integrating the history of one branch into another. Produces either a fast-forward (no new commit) or a merge commit (with two parents). See [[10 Merging]].

**Merge Commit**
A commit with two or more parents, created when non-divergent branches are merged.

**Modified**
One of the three states. A file that has been changed in the working tree but not yet staged. See [[04 The Three States]].

**MOC (Map of Content)**
An Obsidian convention: a note that serves as a navigation hub linking to all notes on a topic, rather than containing content itself.

---

## O

**Object**
The fundamental storage unit in Git's database. Four types: blob, tree, commit, tag. Every object is immutable and identified by its SHA-1 hash. See [[01 Git Object Model]].

**Origin**
The conventional name for the default remote — the repository you cloned from. Just a name; you could call it anything.

---

## P

**Packfile**
A binary file in `.git/objects/pack/` that contains many Git objects compressed together using delta compression. Git creates these automatically to save space. See [[04 Packfiles and GC]].

**Parent**
The previous commit that a commit descends from. Most commits have one parent; merge commits have two or more; the initial commit has none.

**Plumbing**
Low-level Git commands that operate directly on Git objects and refs (`cat-file`, `hash-object`, `ls-tree`). As opposed to "porcelain" (user-friendly commands). See [[05 Plumbing Commands]].

**Porcelain**
High-level, user-friendly Git commands (`commit`, `add`, `push`, `branch`). Built on top of plumbing commands.

**Pull**
`git fetch` + `git merge` (or `git rebase` if configured). Downloads and integrates remote changes. See [[08 Working with Remotes]].

**Pull Request (PR)**
A request to merge a branch into another, typically reviewed by teammates before merging. A GitHub/GitLab feature, not a Git command. See [[10 Team Workflows]].

**Push**
Uploading local commits to a remote repository. See [[08 Working with Remotes]].

---

## R

**Rebase**
Replaying commits from one branch onto another. Creates new commits with the same changes but different parent hashes. Produces linear history. See [[01 Rebase]].

**Ref (Reference)**
A human-readable name for a commit hash. Stored as files in `.git/refs/`. Branches, tags, and HEAD are all refs. See [[03 Refs and HEAD]].

**Reflog**
A local log of every time a branch tip or HEAD moved. Your ultimate recovery tool. Entries are kept for 90 days. See [[06 Reflog]].

**Remote**
A version of the repository hosted elsewhere (GitHub, GitLab, another machine). You interact with it via `git fetch`, `git push`, `git pull`. See [[08 Working with Remotes]].

**Remote Tracking Branch**
A local reference (`origin/main`) that records where a remote branch was the last time you fetched. Read-only — you can't commit to it directly.

**Repository**
A directory tracked by Git. Contains your project files plus the `.git/` database of all history, objects, and config. See [[03 Your First Repository]].

**Reset**
Moving the current branch pointer to a different commit, with varying effects on the staging area and working tree (`--soft`, `--mixed`, `--hard`). See [[07 Undoing Things]].

**Revert**
Creating a new commit that undoes the changes of a previous commit without rewriting history. Safe to use on pushed commits. See [[07 Undoing Things]].

---

## S

**SHA-1**
The cryptographic hash function Git uses to identify objects. Produces a 40-character hex string. Note: Git is migrating to SHA-256 for new repositories.

**Squash**
Combining multiple commits into one. Done via `git merge --squash` or `git rebase -i` with `squash`/`fixup`. See [[02 Interactive Rebase]].

**Staged / Staging Area**
One of the three states. The preparation zone between the working tree and the commit database. Files here are ready for the next commit. See [[04 The Three States]].

**Stash**
Temporarily saving uncommitted changes to a stack, giving you a clean working tree. See [[04 Stash]].

**Submodule**
A Git repository embedded inside another Git repository. The parent records the exact commit of the submodule. See [[08 Submodules]].

---

## T

**Tag**
A named, permanent pointer to a specific commit — usually used to mark releases. Unlike branches, tags don't move. See [[05 Tags]].

**Three-Way Merge**
A merge where Git computes the diff using three commits: the common ancestor, the tip of branch A, and the tip of branch B. Produces a merge commit. See [[10 Merging]].

**Tree**
A Git object type that maps filenames and permissions to blob (file) or tree (directory) hashes. Represents a directory snapshot. See [[01 Git Object Model]].

**Trunk-Based Development**
A workflow where all developers commit directly to `main` (trunk) with very short-lived branches and feature flags. See [[10 Team Workflows]].

---

## U

**Untracked**
A file in the working directory that Git has never been told to track. It won't appear in commits until you `git add` it. See [[04 The Three States]].

**Upstream**
1. The remote branch that a local branch tracks (the default push/pull target).
2. In fork workflows, the original repository you forked from.

---

## W

**Working Tree / Working Directory**
The actual files on your disk — what you see and edit. One of the three sections of a Git project. See [[04 The Three States]].

**Worktree**
`git worktree` allows checking out multiple branches simultaneously into different directories, sharing one `.git/` database.

---

→ [[🏠 Home]]
→ [[⚡ Command Reference]]

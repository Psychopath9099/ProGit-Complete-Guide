---
title: .gitignore
tags: [git, beginner, gitignore]
type: concept-note
difficulty: beginner
official-doc: https://git-scm.com/docs/gitignore
related:
  - "[[05 Staging and Committing]]"
  - "[[03 Your First Repository]]"
---

# .gitignore

## Overview

`.gitignore` is a plain text file that tells Git which files and folders to completely ignore — never track, never stage, never commit. Essential for keeping your repo clean.

---

## Why You Need It

Every project generates files that shouldn't be in version control:

| Category | Examples |
|---|---|
| Build artifacts | `dist/`, `build/`, `*.o`, `*.class` |
| Dependencies | `node_modules/`, `vendor/`, `.venv/` |
| Environment files | `.env`, `.env.local`, `.env.production` |
| IDE settings | `.idea/`, `.vscode/`, `*.swp` |
| OS junk | `.DS_Store` (macOS), `Thumbs.db` (Windows) |
| Logs | `*.log`, `logs/` |
| Secrets | `credentials.json`, `*.pem`, `*.key` |

---

## Syntax

```gitignore
# This is a comment

# Ignore a specific file
.env

# Ignore all files with .log extension
*.log

# Ignore a directory (trailing slash)
node_modules/

# Ignore any file called secret.json anywhere in the project
secret.json

# Ignore build/ in root only (leading slash = root-relative)
/build

# Ignore all .txt files in docs/ and its subdirectories
docs/**/*.txt

# Negate (re-include something that was ignored)
!important.log

# Ignore all .env files except .env.example
.env*
!.env.example
```

---

## Common `.gitignore` Patterns by Project Type

### Node.js
```gitignore
node_modules/
dist/
build/
.env
.env.local
.env.*.local
npm-debug.log*
yarn-error.log*
.DS_Store
coverage/
```

### Python
```gitignore
__pycache__/
*.py[cod]
*$py.class
*.egg-info/
dist/
build/
.venv/
venv/
.env
.pytest_cache/
.coverage
htmlcov/
```

### Java
```gitignore
*.class
*.jar
*.war
target/
.classpath
.project
.settings/
```

### General / All Projects
```gitignore
# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
*.swp
*.swo
*~

# Secrets
.env
*.pem
*.key
credentials.json
```

> [!tip] Use gitignore.io
> Visit [gitignore.io](https://www.toptal.com/developers/gitignore) and enter your tech stack (e.g., "Node, macOS, VSCode") to generate a comprehensive `.gitignore` automatically.

---

## Creating a `.gitignore`

```bash
# Create it in the root of your repo
touch .gitignore
nano .gitignore    # or use any editor
```

Commit it like any other file:
```bash
git add .gitignore
git commit -m "Add .gitignore"
```

---

## Global `.gitignore`

For files you always want to ignore across ALL your projects (like OS files and editor files):

```bash
# Create global gitignore
touch ~/.gitignore_global

# Tell Git to use it
git config --global core.excludesfile ~/.gitignore_global
```

Contents of `~/.gitignore_global`:
```gitignore
.DS_Store
.Trash/
Thumbs.db
.idea/
.vscode/
*.swp
```

---

## Ignoring Already-Tracked Files

`.gitignore` only prevents **untracked** files from being tracked. If you already committed a file and now want to ignore it:

```bash
# 1. Remove it from tracking (but keep it on disk)
git rm --cached .env

# 2. Add it to .gitignore
echo ".env" >> .gitignore

# 3. Commit the removal
git commit -m "Stop tracking .env file"
```

> [!important] This doesn't delete the file from history
> Other people can still see it in old commits. If you accidentally committed secrets, you need to scrub them from history (see Pro Git Chapter 7) and rotate your keys immediately.

---

## Checking What's Ignored

```bash
# See which rule is ignoring a specific file
git check-ignore -v node_modules

# List all ignored files
git ls-files --ignored --exclude-standard
```

---

## `.gitignore` vs `.gitkeep`

Git doesn't track empty directories. To force an empty directory into the repo (e.g., `logs/` that should exist but start empty), create a `.gitkeep` file inside it:

```bash
touch logs/.gitkeep
```

Then in `.gitignore`:
```gitignore
logs/*
!logs/.gitkeep
```

This tracks the directory structure but not its contents.

---

## Common Mistakes

> [!bug] `.gitignore` isn't working for a file
> The file is probably already tracked. Run `git rm --cached <file>` to untrack it.

> [!bug] Added `.gitignore` too late, committed `node_modules/`
> ```bash
> git rm -r --cached node_modules/
> echo "node_modules/" >> .gitignore
> git add .gitignore
> git commit -m "Remove node_modules from tracking"
> ```

> [!bug] Committed `.env` with API keys
> 1. Rotate ALL secrets immediately (assume compromised)
> 2. Remove from tracking: `git rm --cached .env`
> 3. Add to `.gitignore`
> 4. Commit and push

---

## Exercises

1. Create a Node.js project. Add `node_modules/` to `.gitignore`. Run `npm install`. Verify Git doesn't see node_modules.
2. Create a `.env` file with fake credentials. Add to `.gitignore`. Confirm `git status` doesn't show it.
3. Accidentally "track" a file, then use `git rm --cached` to untrack it.

---

## Related Notes

- [[05 Staging and Committing]] — What gets staged vs ignored
- [[03 Your First Repository]] — Set up `.gitignore` at the start

---

## Official References

- [`gitignore` documentation](https://git-scm.com/docs/gitignore)
- [Pro Git: Recording Changes — Ignoring Files](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#_ignoring)
- [gitignore.io generator](https://www.toptal.com/developers/gitignore)
- [GitHub's gitignore templates](https://github.com/github/gitignore)

---

#git/beginner #git/gitignore

# Git Command Reference

A personal cheat sheet of common Git commands and their usage, for quick reference.

## Table of Contents

- [Setup & Configuration](#setup--configuration)
- [Getting Started](#getting-started)
- [Working with Changes](#working-with-changes)
- [Commits](#commits)
- [Branches](#branches)
- [Merging & Rebasing](#merging--rebasing)
- [Undoing Things](#undoing-things)
- [Inspect & Compare](#inspect--compare)
- [Remotes & Collaboration](#remotes--collaboration)
- [Stashing](#stashing)
- [Tags](#tags)
- [Useful Shortcuts](#useful-shortcuts)

---

## Setup & Configuration

```bash
# Set your identity (once per machine)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Use a specific editor for commit messages
git config --global core.editor "code --wait"

# View the current configuration
git config --list

# Help for any command
git help <command>
git <command> --help
```

---

## Getting Started

```bash
# Create a new repository in the current folder
git init

# Clone an existing repository
git clone <repository-url>
git clone <repository-url> <folder-name>

# Show current repository status
git status
```

---

## Working with Changes

```bash
# Stage a file / folder / everything
git add <file>
git add <folder>/
git add .
git add -A          # stage all changes (including deletions)

# Unstage a file (keep the changes)
git restore --staged <file>

# Show changes not yet staged (diff of working tree vs index)
git diff

# Show changes already staged (diff of index vs last commit)
git diff --staged

# Show all tracked files
git ls-files
```

### Status: Short Summary

```bash
git status -s
```

| Symbol | Meaning |
|--------|---------|
| `??`   | Untracked file |
| `A`    | Staged / added |
| `M`    | Modified (first column = staged, second = unstaged) |
| `D`    | Deleted |
| `R`    | Renamed |

---

## Commits

```bash
# Commit staged changes with a message
git commit -m "message"

# Commit with a multi-line message
git commit -m "Title" -m "Body description"

# Stage all tracked changes AND commit (skips git add)
git commit -a -m "message"

# Amend the last commit (change message or add forgotten files)
git commit --amend -m "new message"

# Show commit history
git log
git log --oneline      # compact one-line history
git log --oneline --graph --all   # visual history graph
```

---

## Branches

```bash
# List branches (* marks the current one)
git branch

# List all branches including remote-tracking ones
git branch -a

# Create a new branch
git branch <branch-name>

# Switch to a branch
git checkout <branch-name>
git switch <branch-name>        # newer, clearer syntax

# Create AND switch in one step
git checkout -b <branch-name>
git switch -c <branch-name>

# Rename the current branch
git branch -m <new-name>

# Delete a branch (must not be checked out)
git branch -d <branch-name>     # safe delete (fails if unmerged)
git branch -D <branch-name>     # force delete

# Delete a remote branch
git push origin --delete <branch-name>
```

---

## Merging & Rebasing

```bash
# Merge another branch into the current branch
git checkout <target-branch>     # e.g. main
git merge <feature-branch>

# Abort a merge that has conflicts
git merge --abort

# Rebase current branch on top of another (rewrites history)
git checkout <feature-branch>
git rebase <main-branch>

# Abort a rebase
git rebase --abort

# Interactive rebase (edit, squash, reorder commits)
git rebase -i HEAD~3
```

**After resolving merge/rebase conflicts:**

```bash
git add <resolved-file>
git commit            # for merge
git rebase --continue # for rebase
```

---

## Undoing Things

```bash
# Undo changes to a file in the working tree (restore from index)
git restore <file>
git checkout -- <file>          # older syntax

# Unstage a file (keep the working changes)
git restore --staged <file>

# Reset staging area to match last commit (keep working changes)
git reset

# Move HEAD back N commits, keeping changes
git reset --soft HEAD~1         # keep changes staged
git reset --mixed HEAD~1        # keep changes unstaged (default)

# Move HEAD back N commits and DELETE all changes since (dangerous!)
git reset --hard HEAD~1

# Undo a commit with a new "revert" commit (safe, history preserved)
git revert <commit-hash>

# Undo the last commit but keep its changes in the working tree
git reset --soft HEAD~1
```

> ⚠️ **Warning:** `reset --hard` and `--force` operations destroy history/changes. Make sure you don't need them before running.

---

## Inspect & Compare

```bash
# Show the latest commit of the current branch
git show

# Show a specific commit's changes
git show <commit-hash>

# Show the current branch name
git branch --show-current

# Show which commit last touched a file
git log -p <file>

# Show a file as it was at a specific commit
git show <commit-hash>:<file-path>

# Show changes between two commits
git diff <commitA> <commitB>

# Find a commit by message text
git log --grep="keyword"

# Show who changed what in a file
git blame <file>
```

---

## Remotes & Collaboration

```bash
# List remotes
git remote -v

# Add a remote
git remote add origin <repository-url>

# Remove a remote
git remote remove origin

# Rename a remote
git remote rename <old-name> <new-name>

# Fetch changes from remote (does NOT merge)
git fetch
git fetch origin <branch-name>

# Fetch AND merge the remote branch into your current branch
git pull
git pull origin <branch-name>

# Push local commits to remote
git push
git push origin <branch-name>

# Set upstream so future pulls/pushes track the remote branch
git push -u origin <branch-name>

# Force push (rewrites remote history — use with care)
git push --force
git push --force-with-lease     # safer: refuses if remote moved
```

> **`git pull` = `git fetch` + `git merge`.** When in doubt, `fetch` first and inspect before merging.

---

## Stashing

Temporarily set aside uncommitted changes.

```bash
# Save uncommitted changes
git stash
git stash push -m "optional message"

# List stashes
git stash list

# Re-apply the most recent stash and keep it
git stash apply

# Re-apply AND drop the most recent stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Drop a specific stash
git stash drop stash@{2}

# Drop ALL stashes
git stash clear
```

---

## Tags

```bash
# List tags
git tag

# Create a lightweight tag
git tag <tag-name>

# Create an annotated tag (recommended — stores extra info)
git tag -a <tag-name> -m "release message"

# Push tags to remote
git push origin <tag-name>
git push --tags        # push all tags

# Delete a tag locally
git tag -d <tag-name>

# Delete a tag remotely
git push origin :refs/tags/<tag-name>
```

---

## Useful Shortcuts

```bash
# Full status and diff summary of unstaged changes
git status
git diff --stat

# Discard all local changes in the working tree (dangerous)
git restore .
git checkout -- .

# Remove untracked files/folders (dangerous)
git clean -fd

# Show every branch where a commit/ref is included
git branch --contains <commit-hash>

# Pick a commit from another branch onto the current one
git cherry-pick <commit-hash>
```

---

## Typical Workflow

```bash
# 1. Start a new feature branch
git checkout -b feature/new-thing

# 2. Make changes, then stage and commit
git add .
git commit -m "Add new thing"

# 3. Push and set upstream
git push -u origin feature/new-thing

# 4. Back on the main branch, pull latest, merge the feature
git checkout main
git pull
git merge feature/new-thing
git push

# 5. Delete the merged feature branch
git branch -d feature/new-thing
```

---

*Tip: use `git <command> --help` (or `git help <command>`) to see the full manual for any command.*

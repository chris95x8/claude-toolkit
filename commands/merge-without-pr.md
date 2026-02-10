---
name: merge-without-pr
allowed-tools: Bash(git branch --show-current:*), Bash(git status:*), Bash(git add:*), Bash(git commit:*), Bash(git worktree list:*), Bash(git checkout:*), Bash(git pull:*), Bash(git merge:*), Bash(git push:*)
description: Commit changes in current worktree, merge into main with full history, and push to GitHub
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Active worktrees: !`git worktree list`

## Your task

Based on the above changes:
1. Ensure the current branch is not `main` and not a detached HEAD
2. Stage all changes in the worktree
3. Create a commit containing all changes with an appropriate message
4. Locate the worktree where `main` is checked out
5. Switch to `main` in that worktree
6. Pull the latest changes from `origin/main` using rebase (`git pull --rebase origin main`)
7. Merge the current branch into `main`, preserving all commits
8. Push `main` to origin
9. Do all of the above in a single response using only the allowed tools
10. Do not open a pull request
11. Do not send any text besides the required tool calls
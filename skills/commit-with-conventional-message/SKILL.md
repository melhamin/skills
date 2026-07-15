---
name: commit-with-conventional-message
description: Use when the user asks to commit staged changes and wants a well-formed Conventional Commits message. Inspects the diff and writes a type(scope):-prefixed commit.
---

# Commit with a Conventional Commit message

A small, real example skill. Keep it or delete it.

## When to use

The user asks to "commit", "make a commit", or wants a conventional commit
message for their staged changes.

## Instructions

1. Run `git status` and `git diff --staged` to understand what changed. If
   nothing is staged, ask whether to `git add -A` first.
2. Pick the correct type: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`,
   `perf`, `build`, `ci`, `style`.
3. Derive a short scope from the primary directory or module touched.
4. Write a concise, imperative subject line under ~72 characters:
   `type(scope): subject`.
5. Add a body only when the change needs explanation (the why, not the what).
6. Show the user the message and commit only after they confirm.

---
title: "Git & GitHub Crash Course"
date: 2026-01-11
draft: false
weight: 3
summary: "Learn version control with Git: commits, branches, merges, and collaborating on GitHub."
description: "A beginner-friendly crash course on Git and GitHub for DevOps."
tags: ["git", "github", "version-control"]
categories: ["Version Control"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

Git tracks every change to your code so you can collaborate, experiment safely, and never lose work. GitHub hosts that code online. Together they're the backbone of modern software teams.

## One-time setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Starting a repository

```bash
git init                 # turn a folder into a Git repo
git clone <url>          # copy an existing repo from GitHub
```

## The core workflow

Git has three areas: your **working directory**, the **staging area**, and the **repository**.

```bash
git status               # what's changed?
git add file.txt         # stage a specific file
git add .                # stage everything
git commit -m "message"  # save a snapshot
git log --oneline        # view history
```

Think of it like packing a box: `add` puts items in the box, `commit` seals and labels it.

## Branching

Branches let you work on features without touching the main code.

```bash
git branch feature-login     # create a branch
git checkout feature-login   # switch to it
git checkout -b feature-x    # create AND switch in one step
git merge feature-login      # merge a branch into the current one
```

## Working with GitHub

```bash
git remote add origin <url>      # link your repo to GitHub
git push -u origin main          # upload your commits
git pull                         # fetch and merge remote changes
```

## A typical day-to-day flow

```bash
git checkout -b add-readme       # 1. branch off
echo "# My Project" > README.md  # 2. make changes
git add README.md                # 3. stage
git commit -m "Add README"       # 4. commit
git push -u origin add-readme    # 5. push
# 6. Open a Pull Request on GitHub for review
```

## Pull Requests (PRs)

A Pull Request is how you propose your changes to a project. Teammates review it, leave comments, and approve before it merges into `main`. PRs are where code review and collaboration happen, a core DevOps practice.

## Handy recovery commands

```bash
git diff                 # see unstaged changes
git restore file.txt     # discard changes to a file
git reset --soft HEAD~1  # undo last commit, keep changes
git stash                # shelve changes temporarily
git stash pop            # bring them back
```

## A good .gitignore

Don't commit secrets or junk. Create a `.gitignore`:

```text
node_modules/
.env
*.log
.DS_Store
```

## Practice challenge 🏋️

1. Create a new repo on GitHub.
2. Clone it locally.
3. Add a `README.md`, commit, and push.
4. Create a branch, make a change, push it, and open a Pull Request.

## What's next?

You can track and share code. Now let's automate repetitive tasks with scripting.

👉 Next up: [Bash Scripting for Beginners](/posts/bash-scripting-for-beginners/)

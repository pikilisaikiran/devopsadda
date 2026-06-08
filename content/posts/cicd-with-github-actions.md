---
title: "CI/CD with GitHub Actions"
date: 2026-01-23
draft: false
weight: 7
summary: "Build your first automated pipeline that tests and deploys your code on every push using GitHub Actions."
description: "A hands-on intro to CI/CD pipelines with GitHub Actions."
tags: ["cicd", "github-actions", "automation"]
categories: ["CI/CD"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

CI/CD is the heartbeat of DevOps. It means every time you push code, it automatically gets built, tested, and (optionally) deployed, no manual steps, no human error.

## What CI/CD actually means

- **CI (Continuous Integration)** — automatically build and test your code every time it changes.
- **CD (Continuous Delivery/Deployment)** — automatically release that code to staging or production.

The payoff: catch bugs early, ship small changes often, and sleep better at night.

## Why GitHub Actions?

It's built right into GitHub, free for public repos, and uses simple YAML files. Perfect for learning. The same concepts apply to GitLab CI, Jenkins, and others.

## How it works

Workflows live in `.github/workflows/`. Each workflow has:

- **Events** — what triggers it (e.g., a push)
- **Jobs** — groups of steps that run on a virtual machine (runner)
- **Steps** — individual commands or actions

## Your first workflow

Create `.github/workflows/ci.yml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Check out the code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Commit and push this. Open the **Actions** tab on GitHub, you'll watch it run live. 🎉

## Understanding the pieces

| Keyword | Meaning |
|---------|---------|
| `on` | The event(s) that trigger the workflow |
| `jobs` | One or more jobs that run (in parallel by default) |
| `runs-on` | The OS of the virtual machine |
| `steps` | The ordered commands within a job |
| `uses` | A prebuilt action from the marketplace |
| `run` | A shell command to execute |

## Adding a deploy step

Once tests pass, you can deploy. Here's a job that only runs on `main` after the build succeeds:

```yaml
  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        run: ./deploy.sh
```

`needs` makes `deploy` wait for `build-and-test` to pass first.

## Secrets

Never hardcode passwords or API keys. Store them in **Settings → Secrets and variables → Actions**, then use them:

```yaml
      - name: Deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: ./deploy.sh
```

## Building a Docker image in CI

A very common real-world pipeline:

```yaml
      - name: Build Docker image
        run: docker build -t my-app:${{ github.sha }} .
```

Tagging with `github.sha` gives every build a unique, traceable version.

## Best practices

- Keep pipelines **fast** — slow feedback defeats the purpose.
- Fail loudly — a red X should be impossible to miss.
- Run tests on **pull requests**, not just `main`.
- Cache dependencies to speed up runs.

## Practice challenge 🏋️

Add a workflow to one of your repos that runs a linter on every push. Watch it pass (and intentionally break it to watch it fail).

## What's next?

You can ship automatically. Now let's manage the infrastructure itself as code.

👉 Next up: [Terraform for Beginners](/posts/terraform-for-beginners/)

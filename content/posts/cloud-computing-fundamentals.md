---
title: "Cloud Computing Fundamentals"
date: 2026-01-20
draft: false
weight: 6
summary: "Understand the cloud: compute, storage, networking, IAM, regions, and how to stay on the free tier."
description: "A beginner's guide to cloud computing concepts for DevOps."
tags: ["cloud", "aws", "fundamentals"]
categories: ["Cloud"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

The "cloud" is just someone else's computers that you rent on demand. Instead of buying servers, you pay for compute and storage as you use it. This is where modern DevOps lives.

## Why the cloud?

- **No upfront hardware** — spin up a server in seconds.
- **Pay for what you use** — scale up under load, scale down when quiet.
- **Global reach** — deploy close to your users worldwide.
- **Managed services** — let the provider run databases, queues, and more.

## The three service models

| Model | What you manage | Example |
|-------|-----------------|---------|
| **IaaS** | OS, apps, data | AWS EC2 (virtual machines) |
| **PaaS** | Just your app | Heroku, App Engine |
| **SaaS** | Nothing — just use it | Gmail, Dropbox |

A handy analogy: IaaS is renting a kitchen, PaaS is a meal-kit service, SaaS is ordering takeout.

## The big three providers

- **AWS** — the largest, most services, great free tier. A solid place to start.
- **Azure** — Microsoft's cloud, common in enterprises.
- **GCP** — Google's cloud, strong in data and Kubernetes.

The concepts transfer between all of them. Learn one well.

## Core building blocks (AWS terms)

### Compute

- **EC2** — virtual machines you control.
- **Lambda** — run code without managing servers (serverless).

### Storage

- **S3** — object storage for files, backups, static sites.
- **EBS** — disk volumes attached to EC2.

### Networking

- **VPC** — your private, isolated network in the cloud.
- **Security Groups** — virtual firewalls controlling traffic.

### Identity

- **IAM** — controls *who* can do *what*. Get this right; it's security-critical.

## Regions and availability zones

- A **region** is a geographic location (e.g., `us-east-1`).
- Each region has multiple **availability zones** (isolated data centers).

Spreading across zones keeps your app running even if one data center fails.

## The shared responsibility model

The provider secures the cloud *infrastructure*. **You** are responsible for securing what you put *in* it, your data, access policies, and configurations. Misconfigured permissions are the #1 cause of cloud breaches.

## Staying on the free tier (avoid surprise bills 💸)

- Sign up for the **AWS Free Tier**.
- Use small instance types (`t2.micro` / `t3.micro`).
- **Set a billing alert** the moment you create your account.
- **Stop or terminate** resources when you're done experimenting.

```text
Billing → Budgets → Create a $1 alert. Future-you will be grateful.
```

## Your first hands-on win

1. Create a free AWS account.
2. Launch a `t2.micro` EC2 instance running Ubuntu.
3. SSH into it: `ssh -i key.pem ubuntu@<public-ip>`.
4. Install something (`sudo apt install nginx`).
5. **Terminate the instance** when finished.

## What's next?

You understand where apps run. Now let's automate getting them there with CI/CD.

👉 Next up: [CI/CD with GitHub Actions](/posts/cicd-with-github-actions/)

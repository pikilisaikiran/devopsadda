---
title: "Terraform for Beginners"
date: 2026-01-26
draft: false
weight: 8
summary: "Stop clicking in consoles. Define and provision your cloud infrastructure as code with Terraform."
description: "An introduction to Infrastructure as Code using Terraform."
tags: ["terraform", "iac", "automation", "cloud"]
categories: ["Infrastructure as Code"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

Clicking through a cloud console to create servers works once. But what about doing it consistently, 50 times, across three environments, with a full audit trail? That's where **Infrastructure as Code (IaC)** comes in, and Terraform is the most popular tool for the job.

## What is Infrastructure as Code?

Instead of manually creating resources, you *describe* them in code. Then a tool makes reality match your description. Benefits:

- **Repeatable** — same result every time.
- **Versioned** — your infra lives in Git alongside your app.
- **Reviewable** — changes go through pull requests.
- **Documented** — the code *is* the documentation.

## Why Terraform?

- Works across AWS, Azure, GCP, and 100s of other providers.
- Declarative, you say *what* you want, not *how* to build it.
- Huge community and module ecosystem.

[Install it here](https://developer.hashicorp.com/terraform/install), then check:

```bash
terraform version
```

## The core concepts

| Concept | Meaning |
|---------|---------|
| **Provider** | The platform you're managing (AWS, etc.) |
| **Resource** | A thing you create (a server, bucket, network) |
| **State** | Terraform's record of what it has created |
| **Plan** | A preview of changes before applying |

## Your first configuration

Create a file `main.tf`:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "devops-adda-demo-bucket-12345"

  tags = {
    Name        = "Demo Bucket"
    Environment = "Learning"
  }
}
```

## The core workflow

```bash
terraform init     # download the provider plugins
terraform plan     # preview what will be created/changed
terraform apply    # make it happen (type 'yes' to confirm)
terraform destroy  # tear it all down when you're done
```

Always read the `plan` output before you `apply`. It tells you exactly what's about to change.

## Variables (don't hardcode)

```hcl
variable "region" {
  description = "AWS region to deploy into"
  default     = "us-east-1"
}

provider "aws" {
  region = var.region
}
```

## Outputs

Print useful values after applying:

```hcl
output "bucket_name" {
  value = aws_s3_bucket.my_bucket.bucket
}
```

## Understanding state

Terraform keeps a `terraform.tfstate` file mapping your code to real resources. **Treat it carefully:**

- Never edit it by hand.
- Don't commit it to Git (it can contain secrets).
- On a team, store it remotely (e.g., an S3 backend with locking).

## Best practices

- Use **modules** to reuse configurations.
- Keep environments (dev/staging/prod) separate.
- Run `terraform fmt` to keep code tidy and `terraform validate` to catch errors.
- Always `destroy` learning resources to avoid charges.

## Practice challenge 🏋️

1. Write a config that creates a single S3 bucket.
2. Run `init`, `plan`, then `apply`.
3. Confirm it exists in the AWS console.
4. Run `destroy` to clean up.

## What's next?

You can build infrastructure on demand. Now let's orchestrate all those containers at scale.

👉 Next up: [Kubernetes 101](/posts/kubernetes-101/)

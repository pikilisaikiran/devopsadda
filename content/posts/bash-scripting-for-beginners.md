---
title: "Bash Scripting for Beginners"
date: 2026-01-14
draft: false
weight: 4
summary: "Automate repetitive tasks by writing your first Bash scripts — variables, loops, conditionals, and cron jobs."
description: "Learn Bash scripting fundamentals to automate your DevOps workflows."
tags: ["bash", "scripting", "automation", "linux"]
categories: ["Automation"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

The golden rule of DevOps: **if you do something more than twice, automate it.** Bash scripting is your first automation superpower.

## Your first script

Create a file `hello.sh`:

```bash
#!/bin/bash
echo "Hello, DevOps!"
```

The first line (`#!/bin/bash`) is the *shebang*, it tells the system to run this with Bash. Then make it executable and run it:

```bash
chmod +x hello.sh
./hello.sh
```

## Variables

```bash
#!/bin/bash
name="DevOps Adda"
echo "Welcome to $name"

# Capture command output
today=$(date +%F)
echo "Today is $today"
```

> Use quotes around variables (`"$name"`) to avoid surprises with spaces.

## Taking input

```bash
#!/bin/bash
read -p "What's your name? " user
echo "Hi $user, let's automate something!"
```

## Conditionals

```bash
#!/bin/bash
if [ -f "/etc/passwd" ]; then
    echo "The file exists."
else
    echo "Not found."
fi
```

Common test flags: `-f` (file exists), `-d` (directory exists), `-z` (string empty), `-eq`/`-ne`/`-gt`/`-lt` (number comparisons).

## Loops

```bash
#!/bin/bash
# Loop over a list
for service in nginx docker ssh; do
    echo "Checking $service..."
done

# Loop over files
for file in *.log; do
    echo "Found log: $file"
done

# While loop
count=1
while [ $count -le 3 ]; do
    echo "Attempt $count"
    count=$((count + 1))
done
```

## Functions

```bash
#!/bin/bash
greet() {
    echo "Hello, $1!"   # $1 is the first argument
}

greet "World"
greet "DevOps"
```

## A practical example: a backup script

```bash
#!/bin/bash
SOURCE="/home/user/project"
DEST="/backups"
STAMP=$(date +%Y%m%d-%H%M%S)

mkdir -p "$DEST"
tar -czf "$DEST/backup-$STAMP.tar.gz" "$SOURCE"

echo "Backup created: $DEST/backup-$STAMP.tar.gz"
```

## Scheduling with cron

Run scripts automatically on a schedule:

```bash
crontab -e
```

Add a line (this runs the backup every day at 2 AM):

```text
0 2 * * * /home/user/backup.sh
```

The five fields are: `minute hour day-of-month month day-of-week`. Use [crontab.guru](https://crontab.guru) to decode schedules.

## Good habits

- Add `set -e` near the top so the script stops on the first error.
- Use `set -u` to catch undefined variables.
- Comment your scripts, future-you will thank you.

```bash
#!/bin/bash
set -euo pipefail
```

## Practice challenge 🏋️

Write a script that checks if a website is up:

```bash
#!/bin/bash
read -p "Enter a URL: " url
if curl -s --head "$url" | grep "200 OK" > /dev/null; then
    echo "✅ $url is up!"
else
    echo "❌ $url seems down."
fi
```

## What's next?

You can automate tasks now. Time to learn the technology that changed how we ship software: containers.

👉 Next up: [Docker for Absolute Beginners](/posts/docker-for-beginners/)

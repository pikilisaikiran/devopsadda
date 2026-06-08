---
title: "Linux Basics for DevOps"
date: 2026-01-08
draft: false
weight: 2
summary: "Master the essential Linux commands every DevOps engineer uses daily — files, permissions, processes, and networking."
description: "A hands-on intro to the Linux command line for DevOps beginners."
tags: ["linux", "command-line", "fundamentals"]
categories: ["Linux"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

Servers run Linux. Containers run Linux. CI runners run Linux. If you want to do DevOps, you need to be comfortable at the Linux command line. Let's get you there.

> **Tip:** On Windows? Install [WSL2](https://learn.microsoft.com/windows/wsl/install). On macOS? The Terminal is already Unix-based. Otherwise, spin up an Ubuntu virtual machine.

## Getting around the filesystem

```bash
pwd             # print working directory (where am I?)
ls              # list files
ls -lah         # list with details, including hidden files
cd /etc         # change directory
cd ~            # go to your home directory
cd ..           # go up one level
```

## Working with files and folders

```bash
mkdir projects          # create a directory
touch notes.txt         # create an empty file
cp notes.txt backup.txt # copy
mv backup.txt old.txt   # move or rename
rm old.txt              # delete a file
rm -r projects          # delete a directory and its contents
```

## Reading and editing files

```bash
cat file.txt        # print the whole file
less file.txt       # scroll through a large file (q to quit)
head -n 20 file.txt # first 20 lines
tail -n 20 file.txt # last 20 lines
tail -f app.log     # follow a log in real time
nano file.txt       # simple terminal editor
```

## Permissions (the part that trips everyone up)

Every file has an owner, a group, and permissions for **read (r)**, **write (w)**, and **execute (x)**.

```bash
ls -l script.sh
# -rwxr-xr-- 1 user group 0 Jan 1 12:00 script.sh

chmod +x script.sh   # make a file executable
chmod 644 file.txt   # owner read/write, others read
chown user:group f   # change ownership
```

Quick guide to the numbers: `4 = read`, `2 = write`, `1 = execute`. Add them up per group (owner/group/others). So `755` = `rwxr-xr-x`.

## Processes

```bash
ps aux              # list running processes
top                 # live process view (q to quit)
kill 1234           # stop process with PID 1234
kill -9 1234        # force kill
```

## Networking basics

```bash
ping google.com         # check connectivity
curl https://api.io     # make an HTTP request
curl -I https://site    # headers only
ss -tulpn               # list listening ports
```

## Package management (Ubuntu/Debian)

```bash
sudo apt update             # refresh package lists
sudo apt install nginx      # install a package
sudo apt remove nginx       # remove a package
```

## Piping and redirection

This is where the shell gets powerful, you chain commands together:

```bash
cat access.log | grep "404" | wc -l   # count 404s in a log
ls -l > files.txt                      # write output to a file
echo "hello" >> notes.txt              # append to a file
```

## Practice challenge 🏋️

Try this end to end:

1. Create a folder called `devops-practice`.
2. Inside it, create a file `hello.sh` containing `echo "Hello DevOps"`.
3. Make it executable and run it with `./hello.sh`.

```bash
mkdir devops-practice
cd devops-practice
echo 'echo "Hello DevOps"' > hello.sh
chmod +x hello.sh
./hello.sh
```

If you saw `Hello DevOps` printed, congrats, you just wrote and ran your first script. 🎉

## What's next?

You can move around Linux now. Time to learn how teams track and share code.

👉 Next up: [Git & GitHub Crash Course](/posts/git-github-crash-course/)

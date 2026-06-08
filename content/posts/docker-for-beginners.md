---
title: "Docker for Absolute Beginners"
date: 2026-01-17
draft: false
weight: 5
summary: "Understand containers, write your first Dockerfile, and run multi-container apps with Docker Compose."
description: "A beginner's guide to Docker containers, images, and Docker Compose."
tags: ["docker", "containers", "fundamentals"]
categories: ["Containers"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

"It works on my machine" is the oldest excuse in software. Docker kills it for good by packaging your app *and* everything it needs into a portable **container** that runs the same everywhere.

## Images vs. containers

- An **image** is a blueprint, a snapshot of your app and its dependencies.
- A **container** is a running instance of that image.

Think of an image like a recipe and a container like the actual dish you cook from it. One recipe, many dishes.

## Installing Docker

Grab [Docker Desktop](https://docs.docker.com/get-docker/) (Windows/Mac) or Docker Engine (Linux). Verify it:

```bash
docker --version
docker run hello-world
```

## Essential commands

```bash
docker pull nginx           # download an image
docker images               # list local images
docker run nginx            # run a container
docker run -d -p 8080:80 nginx   # detached, map port 8080 → 80
docker ps                   # list running containers
docker ps -a                # include stopped containers
docker stop <id>            # stop a container
docker rm <id>              # remove a container
docker rmi nginx            # remove an image
docker logs <id>            # view container logs
docker exec -it <id> bash   # open a shell inside a container
```

## Writing your first Dockerfile

A `Dockerfile` describes how to build your image. Here's one for a simple Node.js app:

```dockerfile
# Start from an official base image
FROM node:20-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy dependency files and install
COPY package*.json ./
RUN npm install

# Copy the rest of the app
COPY . .

# Tell Docker which port the app uses
EXPOSE 3000

# The command to run when the container starts
CMD ["node", "server.js"]
```

Build and run it:

```bash
docker build -t my-app .
docker run -d -p 3000:3000 my-app
```

## Layers and caching

Each instruction in a Dockerfile creates a *layer*. Docker caches layers, so copying `package.json` and installing dependencies **before** copying your whole app means dependencies only reinstall when they actually change. That's why the order above matters.

## Docker Compose

Real apps have multiple parts, an app plus a database, for example. Docker Compose runs them together with one file, `docker-compose.yml`:

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

Then:

```bash
docker compose up -d     # start everything
docker compose ps        # see what's running
docker compose down      # stop and clean up
```

## Volumes & persistence

Containers are *ephemeral*, delete one and its data is gone. **Volumes** store data outside the container so it survives restarts (notice `db-data` above).

## Best practices

- Use small base images (like `-alpine`) to keep things lean.
- Never bake secrets into images, use environment variables.
- Add a `.dockerignore` (like `.gitignore`) to skip `node_modules`, `.git`, etc.
- One process per container.

## Practice challenge 🏋️

1. Run an Nginx container mapped to port 8080.
2. Visit `http://localhost:8080` in your browser.
3. Use `docker exec -it <id> bash` to peek inside.

## What's next?

Your app is containerized. Now let's see where it actually runs, the cloud.

👉 Next up: [Cloud Computing Fundamentals](/posts/cloud-computing-fundamentals/)

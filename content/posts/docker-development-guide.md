---
title: "Docker in Development: A Practical Guide"
date: 2024-02-18
draft: false
tags: ["docker", "devops", "tooling"]
description: "How I use Docker to keep local development environments clean, reproducible, and close to production."
---

Docker transformed how I think about local development. Here's the setup that's worked best across different types of projects.

## The Core Principle

Your local environment should be as close to production as possible — without the pain of setting things up from scratch on every machine. Docker lets you encode that environment once and reuse it everywhere.

## A Solid `docker-compose.yml`

For most web projects, I start with three services: the app, a database, and a cache.

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    volumes:
      - .:/app
    environment:
      - DATABASE_URL=postgres://dev:dev@db:5432/myapp
    depends_on:
      - db
      - cache

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: dev
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data

  cache:
    image: redis:7-alpine

volumes:
  pgdata:
```

## Hot Reloading

For Go, I use `air` for hot reload inside the container. For Node/TypeScript, the dev server handles it natively. The key is mounting your source directory as a volume so changes are reflected immediately.

## Multi-Stage Builds for Production

Keep your production image lean with multi-stage builds:

```dockerfile
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o server ./cmd/server

FROM alpine:3.19
COPY --from=builder /app/server /server
EXPOSE 8080
CMD ["/server"]
```

This produces a ~15MB image instead of a ~800MB one. Smaller images mean faster deploys and smaller attack surfaces.

## Closing Thoughts

The initial setup cost of dockerizing a project pays off quickly. New team members can be productive in minutes, CI environments match local ones, and "works on my machine" stops being a phrase anyone says.

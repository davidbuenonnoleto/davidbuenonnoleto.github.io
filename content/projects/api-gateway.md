---
title: "API Gateway"
date: 2024-03-01
draft: false
tags: ["go", "microservices", "backend"]
github: "https://github.com/davidbuenonnoleto"
description: "A lightweight, high-performance API gateway built in Go with rate limiting, auth middleware, and request routing."
---

A lightweight API gateway written in Go, designed to sit in front of microservices and handle cross-cutting concerns like authentication, rate limiting, and request routing.

## Features

- **JWT authentication** middleware with configurable validation
- **Rate limiting** per IP and per API key using a sliding window algorithm
- **Request proxying** with configurable upstream targets and retries
- **Health checks** with automatic upstream failover
- **Structured logging** with request tracing

## Tech Stack

- **Go 1.22** — core service
- **Redis** — rate limit counters and token caching
- **Docker** — containerized deployment

## Architecture

The gateway uses a middleware chain pattern, making it easy to add, remove, or reorder cross-cutting concerns without modifying routing logic.

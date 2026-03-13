---
title: "Building Scalable REST APIs with Go"
date: 2024-03-10
draft: false
tags: ["go", "backend", "api", "architecture"]
description: "Lessons learned designing and shipping production REST APIs in Go — from routing to rate limiting."
---

After building several production REST APIs in Go, I've accumulated a set of patterns that consistently lead to maintainable, performant services. Here's what I wish I knew at the start.

## Start with a Clean Router

Go's standard `net/http` package is powerful, but for anything beyond trivial routing I reach for a lightweight router. My current preference is `chi` — it's idiomatic, composable, and doesn't try to do too much.

```go
r := chi.NewRouter()
r.Use(middleware.Logger)
r.Use(middleware.Recoverer)

r.Route("/api/v1", func(r chi.Router) {
    r.Use(AuthMiddleware)
    r.Get("/users", handlers.ListUsers)
    r.Post("/users", handlers.CreateUser)
})
```

## Error Handling as a First-Class Concern

One of the first things I standardize is error response structure. Clients should always receive a predictable JSON shape, regardless of what went wrong.

```go
type APIError struct {
    Code    string `json:"code"`
    Message string `json:"message"`
    Details any    `json:"details,omitempty"`
}
```

This makes front-end error handling dramatically simpler and keeps your API feeling professional.

## Rate Limiting Early

Don't wait until you need it. Adding rate limiting after the fact is painful. Use middleware from day one — even if the limits are generous initially.

## Structured Logging

Replace `fmt.Println` with structured logging (`slog` from Go 1.21 is excellent). Every request should emit a log line with at least: method, path, status code, latency, and request ID.

## Final Thoughts

The best Go APIs I've seen share a few traits: they're boring in the best way — predictable, explicit, and easy to trace. Resist the urge to add clever abstractions early. Go rewards simplicity.

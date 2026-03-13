---
title: "TypeScript Tips I Use Every Day"
date: 2024-01-25
draft: false
tags: ["typescript", "javascript", "frontend", "dx"]
description: "Practical TypeScript patterns that make codebases easier to maintain and refactor."
---

After a few years of TypeScript across different codebases, certain patterns have become second nature. Here are the ones I reach for most.

## Use `satisfies` for Type-Safe Constants

The `satisfies` operator (TS 4.9+) gives you type checking without widening the type — great for config objects.

```typescript
const config = {
  timeout: 3000,
  retries: 3,
  baseUrl: "https://api.example.com",
} satisfies Record<string, string | number>;

// config.timeout is still `number`, not `string | number`
```

## Discriminated Unions > Optional Properties

Instead of a bag of optional fields, model state explicitly:

```typescript
// ❌ Ambiguous
type Result = { data?: User; error?: string; loading?: boolean };

// ✅ Clear
type Result =
  | { status: "loading" }
  | { status: "success"; data: User }
  | { status: "error"; error: string };
```

The compiler will now enforce exhaustive handling.

## `infer` for Utility Types

When built-in utility types aren't enough, `infer` lets you extract types from complex generics:

```typescript
type UnpackPromise<T> = T extends Promise<infer U> ? U : T;
type ReturnType<T extends (...args: any) => any> =
  T extends (...args: any) => infer R ? R : never;
```

## Keep `as` Casts Rare

Every `as` cast is a place where the compiler can't help you. If you're reaching for it often, it's worth revisiting your types. Sometimes it's unavoidable — external data sources, third-party types — but inside your own code, it's usually a sign to refactor.

## `unknown` Over `any`

Prefer `unknown` when you genuinely don't know the type. Unlike `any`, it forces you to narrow before using it, which keeps safety intact.

These small habits add up to codebases that are significantly easier to maintain and refactor with confidence.

---
title: "DevMetrics Dashboard"
date: 2024-02-01
draft: false
tags: ["react", "typescript", "data viz"]
github: "https://github.com/davidbuenonnoleto"
description: "A real-time engineering metrics dashboard for visualizing CI/CD pipeline performance, deploy frequency, and DORA metrics."
---

A real-time dashboard for engineering teams to track DORA metrics and CI/CD performance over time.

## Features

- **DORA metrics** — deployment frequency, lead time, MTTR, change failure rate
- **CI/CD pipeline visualization** — build time trends, flaky test detection
- **GitHub integration** — pulls data directly from GitHub Actions via API
- **Team-level views** — filter and compare metrics by team or repository

## Tech Stack

- **React + TypeScript** — frontend
- **Recharts** — data visualization
- **FastAPI** — metrics aggregation backend
- **PostgreSQL** — time-series storage

## Motivation

Most engineering teams track these metrics inconsistently or not at all. This dashboard makes DORA metrics visible and actionable with minimal setup time.

---
title: "A Compliance Record Can't Have Gaps"
date: 2026-08-31
draft: false
tags: ["go", "iot", "bluetooth", "architecture"]
description: "Building ChillCheck taught me that one constraint — the log must never have holes — decides most of the interesting design questions."
---

I've been building [ChillCheck](https://github.com/davidbuenonnoleto/chillcheck), a cold-chain
temperature compliance system for restaurants. It replaces the paper clipboard hanging on the
walk-in freezer with automatic monitoring and inspection-ready PDF reports.

The feature list sounds ordinary — sensors, a dashboard, some reports. What made it interesting is
that the artifact isn't the dashboard. It's the **record**. A health inspector reads it, and a gap
in it is not a rendering bug, it's a failed inspection. Once I took that seriously, it stopped
being one requirement among many and started deciding things.

Three decisions came directly out of it.

## 1. The gateway buffers to disk

Cheap BLE temperature sensors broadcast a few meters and have no internet connection of their own.
So there's an agent on-site — a small Go program on a Raspberry Pi — that scans for advertisements,
decodes them, and ships them to the API over HTTPS.

The naive version does `scan → decode → POST`. That version loses data the first time the
restaurant's wifi drops, which in a commercial kitchen is not an edge case.

So the gateway stores and forwards. If the API is unreachable, readings go to a local JSON-lines
spool and get retried on the next cycle. The spool is trimmed to a maximum record count, because an
agent that fills the disk of the box it runs on has traded one outage for a worse one.

## 2. Buffered readings keep their original timestamp

This is the part I'd have gotten wrong if I hadn't been thinking about the inspector.

When a spooled reading finally uploads, the obvious thing is to record it at the time it arrived.
That's easy, and it's wrong. A reading taken at 2:15am during a four-hour outage would land in the
log at 6:30am, and the record would show a four-hour hole followed by an implausible cluster.

So every reading carries the timestamp of when it was *measured*, and the API stores that. Buffered
data lands in the log where it actually happened. The outage becomes invisible in the record, which
is correct — the freezer was being monitored the whole time. The network was the only thing that
failed, and the network isn't what's being audited.

## 3. Not paying doesn't take your data away

ChillCheck bills per location through Stripe, with a trial. The question every SaaS has to answer is
what happens when someone stops paying.

The usual answer is to lock the account. I couldn't do that here. If a restaurant's card expires and
the app stops accepting temperature logs, I haven't applied pressure to upgrade — I've put a hole in
a legal record on their behalf.

So the entitlement check gates *expansion* only. When a trial lapses, you can't add new locations or
units. But logging, reads, sensor ingest, and report export all keep working. You can always get
your compliance data out, whether or not you're a customer.

## The general version

None of these are clever. They're all the same move: find the thing the system actually exists to
produce, and let it decide the questions that look like they're about something else. Retry policy,
timestamp semantics, and billing enforcement don't obviously belong to the same conversation — until
you notice all three can put a hole in the record.

Worth asking on any system: what's the artifact here, and what would it mean for it to be wrong?

The code is on [GitHub](https://github.com/davidbuenonnoleto/chillcheck) — Go API, React frontend,
and the gateway agent, with an architecture writeup.

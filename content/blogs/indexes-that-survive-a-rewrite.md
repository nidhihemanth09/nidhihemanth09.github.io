---
title: Indexes that survive a rewrite
date: 2026-09-12
summary: How to choose indexes from the queries you actually run, and which ones to drop when a schema change makes them useless.
tags:
  - Databases
  - System Design
---

Start from the queries the service runs after the rewrite, not from the indexes that already exist. An index earns its place when it matches the filter and the order of a hot query. If the new predicate cannot use it, the planner will ignore it and the write cost remains.

Drop an index when the new schema makes that access path impossible, even if it used to be the one the planner chose. Keep the one that covers the new filter, and check the plan again after the statistics settle.

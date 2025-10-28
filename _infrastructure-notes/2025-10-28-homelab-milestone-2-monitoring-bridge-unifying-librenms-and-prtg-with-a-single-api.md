---
layout: post
title: "Homelab Milestone 2: Monitoring Bridge: Unifying LibreNMS and PRTG with a Single API"
date: 2025-10-28 00:43:44 +0000
categories: infrastructure homelab
tags: [homelab, infrastructure, automation, ai-orchestration, milestone-2]
phase: "Phase 4: Monitoring Bridge"
milestone: 2
---

# Homelab Milestone 2: Monitoring Bridge: Unifying LibreNMS and PRTG with a Single API

> **Security Note**: All credentials, IP addresses, MAC addresses, and sensitive
> information in this post are **fictional examples** sanitized for publication.
> Your environment will have different values. Never share real credentials publicly.

## Phase: Phase 4: Monitoring Bridge

## What We Built

Container 152 with Python 3.11, FastAPI REST API, async clients for both platforms, device merger with deduplication, consensus engine, Redis caching, and systemd service. 19 Python files totaling 2000 lines of code.

## Challenges Encountered

Module import issues during deployment, PRTG API endpoint discovery (404 errors), different authentication methods between platforms, and device deduplication complexity across different hostname formats.

## Solutions

Built async API clients with shared base class, implemented priority-based device matching (IP > hostname > MAC), created consensus algorithm for status conflicts, added Redis caching with in-memory fallback, and deployed as systemd service for reliability.

## Lessons Learned

Start simple then optimize, use async/await throughout for performance, Pydantic validation catches bugs early, FastAPI auto-generates API docs, test happy path first then edge cases, systemd makes everything production-ready, and dual monitoring philosophy validated with automatic failover.

## What's Next?

Implement alert merging endpoint, add device sensor aggregation, WebSocket support for live updates, API authentication, VLAN 100 migration, and AI Orchestrator deployment.

---

## Project Series

This is **Milestone 2** in my AI-Powered Home Lab series.

**Previous posts:**
- [Part 1: Architecture & Planning](link-to-part-1)

**Coming soon:**
- Next milestone post

---

*Automatically logged from LLM Infrastructure Orchestrator project*
*Blog automation powered by Claude Desktop + Code*

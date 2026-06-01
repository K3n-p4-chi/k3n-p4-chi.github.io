---
layout: post
title: "Phase 1: Building the Memory Layer Before Automation"
date: 2026-03-12 20:30:00 +0000
categories: [home-projects]
tags: [homelab, ai-agents, documentation, monitoring, automation]
summary: "Before the agents could operate the lab safely, Phase 1 had to turn a messy environment into a documented, queryable, and shared source of truth."
---

The part that sounds impressive in this project is the AI control plane.

The part that actually made it work was Phase 1.

Before Codex and GitHub Chat could do useful infrastructure work in VS Code, the environment had to stop being a collection of half-remembered settings, old notes, scattered credentials, and inconsistent names. The agents needed a memory layer first.

That was the real purpose of Phase 1: infrastructure discovery.

It was not glamorous. It was the work of finding out what existed, how it was connected, how it was monitored, which names were wrong, which access paths were safe, and which parts of the network could be trusted as reference points for later automation.

## What Phase 1 Actually Delivered

By the end of Phase 1, the project had produced:

- a usable infrastructure inventory
- dual-monitoring parity for the core estate
- documented VLAN and routing groundwork
- access hardening and SSH key deployment work
- Ruckus audit material
- baseline configs and backups for the main systems
- a growing repository structure for status, handoff, topology, and task execution

In the project docs, the summary is blunt: Phase 1 was about discovering, documenting, and stabilising the environment so later automation and migration work could happen reliably.

That sounds obvious. It is also the step most people want to skip.

## Why This Phase Mattered

AI agents are very good at moving once the rails exist.

They are much less safe when the environment itself is ambiguous.

If a repo cannot answer simple questions like these, then autonomous or semi-autonomous operations are mostly theatre:

- Which systems are active right now?
- Which ones are retired and should never be targeted?
- Which IPs, VLANs, and hostnames are authoritative?
- Which credentials are valid, and from which host paths?
- Which commands are known-safe and repeatable?
- Which task is current, and which document is historical?

Phase 1 started building those answers.

## The Monitoring Breakthrough

One of the most useful outcomes was monitoring parity across the core infrastructure.

The auto-generated inventory summary from that period shows the shape of the estate clearly:

- 11 core devices monitored by both LibreNMS and PRTG
- 68 devices visible in PRTG
- 347 PRTG sensors
- zero LibreNMS-only devices in the core set

That dual-monitoring approach mattered because it created confidence in the inventory. It was not just one tool claiming the network looked a certain way. The core devices were being observed from two different systems with two different strengths: PRTG for availability and breadth, LibreNMS for deeper infrastructure metrics.

That was an early version of the same pattern the later agent workflow uses everywhere else: do not trust a single perspective when two are available.

## Discovery Became Structure

The other important Phase 1 outcome was structural rather than technical.

The project stopped being a loose collection of chats and started becoming a repository with roles:

- topology and registry files for infrastructure truth
- handoff and status files for session continuity
- task folders for execution history
- scripts and cookbooks for repeatable actions
- vault-backed credential references instead of ad hoc notes

Older Phase 1 preparation notes still show the shape of that transition. Even at that stage, the work was already heading toward the same pattern the project uses now: a shared workspace where one session leaves enough context behind for the next one to continue safely.

That continuity is what made the later multi-agent workflow viable.

## Naming, Access, and Reality Checks

Phase 1 also did a lot of the thankless work that later phases depend on:

- aligning names between systems
- documenting network topology and planned allocations
- collecting baseline configs and backups
- improving unattended access paths
- separating active infrastructure from future or retired plans

Without that, Phase 2 would have been much harder. Proxmox changes, OPNsense work, CT411 rebuilds, and migration planning all depend on being able to trust the repo more than memory.

That trust does not happen automatically. It is built one inventory file, one verified access path, and one corrected note at a time.

## The Real Output of Phase 1

If I had to summarise Phase 1 in one sentence, it would be this:

Phase 1 turned the homelab into something that could be described consistently enough for machines to help operate it.

Not fully automate it. Not magically run it. Just describe it well enough that AI agents could enter the picture without immediately becoming dangerous.

That is a much more useful milestone than it first sounds.

Most automation failures do not begin with a bad command. They begin with bad context.

Phase 1 reduced that problem.

## What Came Next

Once the memory layer existed, the project could move into more operational work:

- Proxmox deployment and migration planning
- OPNsense changes and HA work
- CT411 rebuild and validation
- host-specific startup routines
- enforced handoff logging
- cookbooks and local indexes that let sessions resume from different machines

That is where the project starts to look like an AI-operated control plane.

But the reason it could get there is much less dramatic.

Phase 1 did the slow work first.

It made the environment legible.

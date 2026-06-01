---
layout: post
title: "How VS Code Became the Control Plane for My AI-Operated Homelab"
date: 2026-03-11 22:00:00 +0000
categories: [home-projects]
tags: [vscode, ai-agents, homelab, ssh, automation]
summary: "An opening draft for the story of how VS Code became the control plane for a multi-agent AI-operated homelab."
---

Most people use VS Code to write code.

I use it to operate a distributed AI-managed homelab.

Not metaphorically. Literally.

My editor has become a control plane where two different large language model agents, GitHub Chat and Codex, work side by side to configure, repair, and optimise machines across my home network. They do not just suggest commands. They execute them, inspect the results, decide the next step, and coordinate through a shared repository that acts as memory, toolbox, and message bus.

There is no Kubernetes layer behind it. No orchestration platform. No complex agent framework.

The system is held together with SSH keys, tightly scoped accounts, shared project files, and a very opinionated operational repository.

At the centre of that repository is `LLM_Handoff.md`, a live session log that every agent reads before starting work and updates when it finishes. It records what was attempted, what succeeded, what failed, and what the next agent needs to do. That is what allows two independent AI systems to touch the same infrastructure without colliding blindly.

But the handoff file is only one part of the control plane.

The repository now contains:

- `AGENTS.md`, which acts like a startup and operating manual for the agents
- `PROJECT-STATUS.md`, which defines the current phase and the authoritative task in flight
- `Topology.md`, `IP-Registry.md`, and `Device-Access-Matrix.md`, which describe what exists, what is active, and how it can be reached
- host-specific local indexes so the same workflow can run from different machines
- cookbooks for WSL, API functions, and VS Code chat so successful commands become reusable procedures instead of one-off terminal history
- small enforcement scripts, including the handoff entry logger and the archive script that keeps the live log readable

That combination is what turned VS Code from an editor into an operating surface. The AI is not floating around in a vague prompt-only environment. It is booting into a documented system with a startup sequence, a memory model, a set of guard rails, and a current view of the network.

The infrastructure it controls is not hypothetical either. The current live environment includes Proxmox on `pve-node2`, OPNsense on `192.168.100.154`, the rebuilt CT411 media stack on `192.168.41.61`, Synology NAS systems on VLAN 41, and the supporting switch, AP, and recovery VM estate around them. In practice, the agents move between SSH, APIs, local scripts, and documentation updates as part of the same session.

That matters because the hard part of using AI for infrastructure is not generating commands. It is maintaining continuity. One agent has to know what the other one already changed. It has to know which systems are active, which ones are retired, which credentials are valid, which cookbook command is the safe one, and which phase document is current instead of historical.

This repo solves that with discipline rather than sophistication.

There is a mandatory startup sequence. There is a mandatory handoff log. There are explicit rules for credentials, cookbook updates, and phase tracking. If the context is incomplete, the session is supposed to stop. That sounds heavy for a personal setup, but it is the reason this arrangement has stayed useful instead of degenerating into AI-assisted chaos.

Over time, the repository has evolved into more than a log. It has become a growing library of scripts, troubleshooting playbooks, configuration templates, and operational conventions, much of it produced and refined by the agents themselves. They read the same files, use the same tools, and inherit the same context.

The result is a multi-agent operating model that is far more practical than it sounds. It has already been used to manage Proxmox work, rebuild and validate CT411, track TR1920 migration state, verify certificate trust for internal services, and coordinate Plex- and OPNsense-related operations through a workflow that is lightweight, auditable, and surprisingly effective.

This series is the story of how that system came to exist: how two unrelated AI models, running in the same editor, became cooperative operators in a real homelab, and why that accidental architecture may point toward a future model for managing personal infrastructure.

The next posts should unpack that properly: how the repository became the shared memory layer, how the startup protocol emerged, how handoffs prevented collisions, and how that structure turned ordinary editor sessions into something much closer to an operator console.

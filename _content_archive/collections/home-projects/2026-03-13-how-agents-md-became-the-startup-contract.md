---
layout: post
title: "How AGENTS.md Became the Startup Contract"
date: 2026-03-13 20:30:00 +0000
categories: [home-projects]
tags: [homelab, ai-agents, documentation, vscode, operations]
summary: "The control plane only became reliable when the agents stopped improvising startup and began booting into a strict contract."
---

If the handoff log is the heartbeat of this project, then `AGENTS.md` is the boot sequence.

It is the file that turned a clever workflow into a disciplined one.

At first, the idea seemed simple enough: give an AI agent access to the repo, let it read some infrastructure notes, and ask it to continue from where the last session left off.

That works right up to the point where it does not.

The problem is not that the model forgets everything. The problem is that it will confidently proceed with partial context unless you force it not to.

That is why `AGENTS.md` exists.

## The Real Problem It Solves

In a multi-agent setup, the dangerous failure mode is not usually a dramatic command. It is a quiet assumption.

An agent assumes it is running in one environment when it is actually in another. It assumes a host is reachable because it saw a stale document. It assumes a file path is valid on the current machine. It assumes a phase runbook is authoritative when a newer emergency override has replaced it.

That kind of drift is exactly how small infrastructure mistakes turn into confusing sessions.

`AGENTS.md` is there to reduce that drift before any real work starts.

## It Starts With Identity, Not Work

One of the first things the startup contract does is force the agent to identify itself based on available tools, not based on whatever the system prompt happens to claim.

That sounds minor. It is not.

If one session is effectively WSL Codex and another is VS Code GitHub Chat with PowerShell and Perplexity available, they do not have the same execution model. They do not have the same shell. They do not have the same local paths. They do not even have the same assumptions about which helper tools should exist.

So the contract makes the agent prove who it is first.

That is a pattern I now trust more broadly: in infrastructure work, identity should be discovered from capability, not declared from vibes.

## Then It Forces Environmental Reality Checks

The startup sequence does not jump straight into the task.

It makes the agent establish:

- hostname
- timestamp
- network context
- detected VLAN
- certificate trust where required
- Perplexity MCP readiness
- local and shared path roots

This matters because a homelab is not one machine. It is a set of vantage points.

The same repo behaves differently depending on whether the work is happening from the admin laptop, from WSL, or from another host entirely. A host-local index, a vault path, and an external workspace path all need to be interpreted correctly before anything else is safe.

That is why the contract stores values like `CURRENT_HOST`, `SESSION_ID`, `BASE_PATH`, and now `BLOG_BASE_PATH` during startup. It is not ceremony for its own sake. It is session anchoring.

## The Most Important Rule: Read Before Acting

The part of `AGENTS.md` that changed the workflow most is the mandatory file-read sequence.

Before doing real work, the agent has to load the current project state from the files that matter:

- the global index
- the local host index
- topology and IP registry
- the device access matrix
- authentication guidance
- project status
- the live handoff log
- cookbooks
- phase documents
- migration-critical ledgers

More recently, I extended that idea to the linked blog workspace as well, because the documentation effort is now part of the operational environment rather than a separate afterthought.

That file-read gate exists for one reason: context should be loaded deliberately, not assumed opportunistically.

## It Is a Contract Because It Has Stop Conditions

The most useful thing about `AGENTS.md` is not that it tells the agent what to read.

It is that it tells the agent when to stop.

If critical files are missing, it should stop.
If identity and actual tool access do not match, it should stop.
If the validation step cannot confirm the current phase and active infrastructure, it should stop.
If handoff logging fails after the task, the task is not considered complete.

That is what makes it a contract instead of a checklist.

A checklist is easy to ignore.

A contract defines failure conditions.

## Why Strictness Turned Out to Be Useful

From the outside, `AGENTS.md` can look excessive. It is opinionated, repetitive, and full of hard stops, exact output blocks, and mandatory verification steps.

That is deliberate.

The stricter the startup, the less time gets wasted later untangling bad assumptions.

In practice, that strictness has done a few important things for the project:

- it made sessions resumable across different hosts
- it reduced silent drift between documentation and action
- it forced the repo to become a real source of truth
- it gave both agents the same startup shape even when their execution environments differ
- it made operational blogging easier because the structure of the work is already documented

The contract does not make the agents smarter.

It makes them easier to trust.

## The Hidden Benefit: It Changed the Repo Itself

Once the agents had to read the same files every time, those files became more valuable.

`PROJECT-STATUS.md` had to become clearer.
The topology had to stay current.
The access matrix had to separate unattended systems from manual ones.
The handoff log had to stay readable.
The local indexes had to reflect reality on each host.

In other words, the startup contract did not just shape agent behaviour.

It shaped the documentation standard of the project.

That is probably the most useful side effect of all.

## Why I Think This Pattern Scales

I do not think personal infrastructure needs enterprise process for its own sake.

But I do think AI-assisted operations need a stronger startup model than most people expect.

Prompting a model with “continue from last time” is not enough.

Giving it a startup contract that defines identity, environment, required context, validation, and completion criteria is much closer to what actually works.

That is what `AGENTS.md` became in this project.

Not a note to the model.

A contract for entering the system.

And once that contract existed, the rest of the control plane got a lot more reliable.

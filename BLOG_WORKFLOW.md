# Blog Workflow From VSCode

This repo is the source for the live blog at `https://k3n-p4-chi.github.io`.

## Site Taxonomy

- `continuous-learning.md`: public page title is `My Upskill Journey`
- `study-notes.md`: landing page for study-session and certification notes
- `cybersecurity-labs.md`: landing page for security-focused lab projects
- `home-projects.md`: landing page for standalone home projects
- `skills-applied-at-work.md`: landing page for work-relevant technical applications
- `planning-tools.md`: landing page for planning sessions, scripts, and tracker work
- `_study-notes/`: actual study entries
- `_cybersecurity-labs/`: actual cybersecurity lab entries
- `_home-projects/`: actual home project entries
- `_work-applied/`: actual work-applied entries
- `_planning-tools/`: actual planning and tooling entries

## Entry Rules

Each new entry is just one Markdown file in the correct collection folder.

### Study note
Path:
`_study-notes/YYYY-MM-DD-short-title.md`

### Planning or tooling post
Path:
`_planning-tools/YYYY-MM-DD-short-title.md`

### Cybersecurity lab post
Path:
`_cybersecurity-labs/YYYY-MM-DD-short-title.md`

### Home project post
Path:
`_home-projects/YYYY-MM-DD-short-title.md`

### Skills applied at work post
Path:
`_work-applied/YYYY-MM-DD-short-title.md`

## Front Matter Template

Use this at the top of every new file:

```md
---
layout: post
title: "Your title here"
date: 2026-03-11 22:30:00 +0000
categories: [study-notes]
tags: [security-plus, domain-1]
summary: "One-line summary for listing pages."
---
```

Use categories like this:

- Study entry: `categories: [study-notes]`
- Planning entry: `categories: [planning-tools]`
- Cybersecurity lab entry: `categories: [cybersecurity-labs]`
- Home project entry: `categories: [home-projects]`
- Work-applied entry: `categories: [skills-applied-at-work]`

Tags are the detail layer. Keep them short and reusable, for example:

- `security-plus`
- `md-102`
- `powershell`
- `homelab`
- `meraki`
- `automation`

## Minimum Safe Workflow

Open a terminal in this repo:

```bash
cd "/mnt/c/Users/frenc/OneDrive/Upskill/Branch 3 - Blog/k3n-p4-chi-blog"
```

Check what changed:

```bash
git status
```

Stage only the files you want in the next commit:

```bash
git add _study-notes/2026-03-11-security-plus-domain-1-week-1.md
git add study-notes.md
```

Create the commit:

```bash
git commit -m "Add Security+ Domain 1 week 1 study note"
```

Push to GitHub:

```bash
git push origin main
```

## What Each Git Command Means

- `git status`: shows changed files
- `git add <file>`: marks specific files for the next commit
- `git commit -m "message"`: saves a checkpoint locally
- `git push origin main`: sends your local commits to GitHub and triggers site update

## Important Safety Rule

Do not use `git add .` unless you have already checked `git status` and you are sure every changed file should go live.

This repo already gets unrelated edits sometimes. Stage specific files, not everything.

## Publish Expectation

After `git push origin main`, GitHub Pages usually updates the site within a minute or two.

## If Push Fails

Read `SETUP_GITHUB_AUTH.md`.

The normal fix is:

1. Create a GitHub Personal Access Token
2. Run `git push`
3. Enter username `k3n-p4-chi`
4. Paste the token as the password

---
layout: page
title: Planning & Tools
permalink: /planning-tools/
---

# Planning & Tools

This section covers the operational side of the journey: planning sessions, automation work, tracker improvements, and the systems used to keep the upskilling project structured.

## What Belongs Here

- Schedule design sessions
- Tracker and automation updates
- Script design notes
- Process fixes and workflow improvements

## Core Themes

- Planning discipline
- Repeatable systems
- Automation over manual repetition
- Accurate source data and auditability

## Latest Entries

{% assign entries = site["planning-tools"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No planning or tooling entries have been published yet.
{% endif %}

## Typical Post Types

- Planning session logs
- Script implementation notes
- Debugging and architecture write-ups
- Workflow changes that affect study delivery

---

*This is the systems-engineering layer behind the public study journey.*

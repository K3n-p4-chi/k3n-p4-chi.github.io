---
layout: page
title: Study Notes & Learning
permalink: /study-notes/
---

# Study Notes & Learning

This section is the working record of certification study, revision blocks, and concept consolidation. It is where the real learning trail lives: what was studied, what clicked, what needs review, and how the current track is progressing.

## Current Focus

- Security+
- Python PCEP
- PowerShell support work
- Microsoft track preparation

## What Belongs Here

- Domain-by-domain certification notes
- Revision session write-ups
- Quiz reflections and score analysis
- Practical takeaways from labs or guided exercises

## Latest Entries

{% assign entries = site["study-notes"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No study notes have been published yet.
{% endif %}

## Writing Standard

Each entry should answer four questions quickly:

- What did I study?
- What did I understand?
- What remains weak?
- What is the next action?

---

*This section should stay factual and cumulative. It is the evidence trail for the learning process.*

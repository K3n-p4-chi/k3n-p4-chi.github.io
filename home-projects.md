---
layout: page
title: Home Projects
permalink: /home-projects/
---

# Home Projects

This section is for standalone build work done at home that is broader than lab notes. These are project-style entries with a clear problem, architecture, implementation path, and outcome.

## Planned Subsections

- Using VS Code as my multi-agent AI-operated SSH control plane

## Latest Entries

{% assign entries = site["home-projects"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No home project entries have been published yet.
{% endif %}

## Writing Standard

Each post should cover:

- the problem being solved
- the target setup
- the tools and architecture
- the implementation steps
- the final outcome

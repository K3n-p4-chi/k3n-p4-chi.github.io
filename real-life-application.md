---
layout: page
title: CyberSec Documented Home Labs
permalink: /home-labs/
---

# CyberSec Documented Home Labs

This section covers the infrastructure side of the journey: home lab design, monitoring, network changes, audits, and the practical systems work that feeds the broader AI-operated environment.

## What Belongs Here

- monitoring milestones
- VLAN and routing changes
- firewall and switching work
- audit write-ups
- recovery notes and infrastructure decisions

## Latest Entries

{% assign entries = site["infrastructure-notes"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No infrastructure notes have been published yet.
{% endif %}

## Writing Standard

The strongest posts here explain:

- what changed in the environment
- why the change was needed
- what problems appeared during execution
- what the final operational state became

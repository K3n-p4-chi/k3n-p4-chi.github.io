---
layout: page
title: CyberSec Documented Home Labs
permalink: /home-labs/
---

# CyberSec Documented Home Labs

This section is the lab record for the upskill journey. It is separate enough to stand on its own as portfolio evidence, but it still supports the main learning track rather than replacing it.

## What Belongs Here

- Home lab build logs
- SIEM and monitoring experiments
- Virtual networking and traffic analysis
- Security tooling deployment notes
- Repeatable lab setups and findings

## Planned Subsections

- Splunk
- Basic SIEM environment
- Virtual network labs
- Traffic monitoring and inspection

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
No real-life application entries have been published yet.
{% endif %}

## Writing Standard

The strongest entries in this section explain:

- The problem
- The environment
- The implementation
- The result or impact
- What changed after the first version

---

*Treat this section as portfolio-grade lab evidence, not casual notes.*

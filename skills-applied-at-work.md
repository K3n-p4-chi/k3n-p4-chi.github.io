---
layout: page
title: Skills Applied at Work
permalink: /skills-applied-at-work/
---

# Skills Applied at Work

This section is for practical work-relevant outcomes where learning turned into something useful, efficient, or measurable in a professional environment.

## Planned Subsections

- MAC Address extractor
- Deployment optimisation

## Latest Entries

{% assign entries = site["work-applied"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No work-applied entries have been published yet.
{% endif %}

## Writing Standard

The strongest entries here show:

- the business problem
- the action taken
- the technical skill applied
- the result for the team or environment

---
layout: page
title: Cybersecurity Labs
permalink: /cybersecurity-labs/
---

# Cybersecurity Labs

This section contains security-focused build work and lab projects. It is where defensive engineering, monitoring, traffic analysis, and practical security environments are documented as structured projects rather than loose notes.

## What Belongs Here

- Hardening my home network
- Monitoring and defensive visibility projects
- SIEM and log-ingestion environments
- Virtual network labs with traffic inspection
- Splunk practice and related lab work

## Current Project Track

### Project A: Hardening My Network
This is the first major cybersecurity project in the section.

Its early phases include the monitoring and orchestration groundwork that was originally published as homelab infrastructure milestones. Those posts now sit here because they form the foundation of the wider network-hardening effort.

## Planned Subprojects

- Hardening my home network
- Basic SIEM environment
- VirtualBox network and traffic-monitoring lab
- Splunk environment practice

## Latest Entries

{% assign entries = site["cybersecurity-labs"] | sort: "date" | reverse %}
{% if entries.size > 0 %}
{% for post in entries %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
No cybersecurity lab posts have been published yet.
{% endif %}

## Writing Standard

The strongest entries in this section explain:

- the security problem or goal
- the environment and constraints
- the implementation steps
- the validation method
- the security outcome or lesson learned

---

*This section is for security-focused project work with clear technical intent and traceable outcomes.*

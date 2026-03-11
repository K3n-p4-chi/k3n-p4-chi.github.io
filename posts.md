---
layout: page
title: All Posts
permalink: /posts/
---

# All Blog Posts

{% assign combined = site["study-notes"] | concat: site["planning-tools"] | concat: site["cybersecurity-labs"] | concat: site["home-projects"] | concat: site["work-applied"] | sort: "date" | reverse %}
{% if combined.size > 0 %}
{% for post in combined %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.summary | default: post.excerpt }}

**Section:** {{ post.section | default: "General" }}  
**Categories:** {{ post.categories | join: ", " }}  
**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% else %}
{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt }}

**Categories:** {{ post.categories | join: ", " }}  
**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}
{% endif %}

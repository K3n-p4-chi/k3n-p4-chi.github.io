---
layout: page
title: All Posts
permalink: /posts/
---

# All Blog Posts

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt }}

**Categories:** {{ post.categories | join: ", " }}  
**Tags:** {{ post.tags | join: ", " }}

---
{% endfor %}

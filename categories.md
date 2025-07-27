---
layout: page
title: Categories
permalink: /categories/
---

# Content Categories

## 📚 **Certification Journey**
Detailed study plans, practice test results, and exam experiences for Azure, Cisco, and CompTIA certifications.

## 🔧 **Technical Projects**
Automation scripts, home lab configurations, and practical cloud security implementations.

## 💼 **Career Development**
Job applications, interview experiences, professional networking, and industry insights.

## 🏠 **Home Lab**
Network configurations, security implementations, and infrastructure experiments.

## 📊 **Progress Tracking**
Weekly dashboards, milestone reviews, and methodology refinements.

## 💡 **Learning Resources**
Course reviews, useful tools, documentation, and community recommendations.

---

{% for category in site.categories %}
## {{ category[0] | capitalize }}
{% for post in category[1] %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%d %B %Y" }}
{% endfor %}
{% endfor %}

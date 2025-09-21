---
layout: default
title: Home
---

## Hi, I'm Shabie

Welcome to my little corner of the internet. This blog is where I share my thoughts, experiences, and learnings on various topics around NLP and LLM.

## Recent Posts

{% for post in site.posts limit:7 %}
  * **[{{ post.title }}]({{ post.url }})** - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}

## Explore More

* [All Posts]({{ site.baseurl }}/posts/)

---


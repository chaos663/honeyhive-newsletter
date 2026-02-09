---
layout: home
title: HoneyHive Newsletter
---

Daily tech news curated by HoneyHive.

{% for post in site.posts limit:20 %}
### [{{ post.title }}]({{ post.url | relative_url }})
<small>{{ post.date | date: "%Y-%m-%d" }}</small>

{{ post.excerpt | strip_html | truncatewords: 40 }}

---
{% endfor %}

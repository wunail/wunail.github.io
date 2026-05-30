---
layout: single
title: "标签"
permalink: /tags/
author_profile: true
---

{% assign tags_sorted = site.tags | sort %}
{% for tag in tags_sorted %}
{% assign tag_name = tag[0] %}
{% assign posts = tag[1] %}

## {{ tag_name }} ({{ posts.size }})

{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) <small>{{ post.date | date: "%Y-%m-%d" }}</small>
{% endfor %}

{% endfor %}

---
layout: single
title: "分类"
permalink: /categories/
author_profile: true
---

{% assign categories_sorted = site.categories | sort %}
{% for cat in categories_sorted %}
{% assign category_name = cat[0] %}
{% assign posts = cat[1] %}

## {{ category_name }} ({{ posts.size }})

{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) <small>{{ post.date | date: "%Y-%m-%d" }}</small>
{% endfor %}

{% endfor %}

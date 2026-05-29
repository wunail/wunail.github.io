---
layout: home
title: 首页
---

# 欢迎来到我的博客 🎉

这里记录技术探索、学习笔记和生活感悟。

---

## 📝 最新文章

{% for post in site.posts limit:10 %}
<article class="post-item">
  <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
  <div class="post-meta">
    <time>{{ post.date | date: "%Y-%m-%d" }}</time>
    {% if post.category %}<span class="category">📂 {{ post.category }}</span>{% endif %}
  </div>
  <p class="post-excerpt">{{ post.description | default: post.excerpt | strip_html | truncate: 150 }}</p>
  <div class="post-tags">
    {% for tag in post.tags %}
    <span class="tag">#{{ tag }}</span>
    {% endfor %}
  </div>
</article>
{% endfor %}

---

## 🏷️ 按分类浏览

<div class="categories">
{% for category in site.categories %}
<a href="{{ category[0] | slugify | prepend: '/category/' | prepend: site.baseurl }}" class="category-link">
  📂 {{ category[0] }} ({{ category[1].size }})
</a>
{% endfor %}
</div>

---

## 📊 博客统计

- 📝 文章总数：{{ site.posts.size }}
- 🏷️ 标签总数：{{ site.tags.size }}
- 📂 分类总数：{{ site.categories.size }}

---

*最后更新：{{ site.time | date: "%Y-%m-%d" }}*

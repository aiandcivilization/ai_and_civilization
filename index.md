---
title: Welcome
---

# AI & Civilization

Welcome to my blog where I explore how Artificial Intelligence and optimization techniques are reshaping our world.

Here are all my writings ↓

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>

---
layout: page
title: Blog
---

Welcome to my coding notes and experiments.

---

##  All Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span style="color:gray; font-size:0.9em;">
        ({{ post.date | date: "%B %d, %Y" }})
      </span>
    </li>
  {% endfor %}
</ul>



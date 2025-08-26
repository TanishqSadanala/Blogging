---
layout: page
title: Blog
---

Welcome to my coding notes and experiments.

## Posts
- [Pygame Notes — Part 1](_posts/2025-08-23-pygame-notes-part1.md)
---
layout: page
title: "Blog"
permalink: /blog/
---

Welcome to my coding notes and experiments.  
Here you’ll find all posts, ordered by date (newest first).

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

---

## 🏷 Categories

{% for category in site.categories %}
### {{ category[0] | capitalize }}
<ul>
  {% for post in category[1] %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span style="color:gray; font-size:0.9em;">
        ({{ post.date | date: "%B %d, %Y" }})
      </span>
    </li>
  {% endfor %}
</ul>
{% endfor %}

---

## 🔎 Tags (optional)

{% for tag in site.tags %}
### {{ tag[0] }}
<ul>
  {% for post in tag[1] %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span style="color:gray; font-size:0.9em;">
        ({{ post.date | date: "%B %d, %Y" }})
      </span>
    </li>
  {% endfor %}
</ul>
{% endfor %}


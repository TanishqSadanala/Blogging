---
layout: page
title: Blog
nav_order: 2
---



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


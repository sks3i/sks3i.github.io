---
layout: page
title: Cooking
---

<div class="card-list">
  {% for post in site.cooking %}
    <div class="card">
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt | strip_html | truncate: 100 }}</p>
      <span class="date">{{ post.date | date: "%B %d, %Y" }}</span>
    </div>
  {% endfor %}
</div> 
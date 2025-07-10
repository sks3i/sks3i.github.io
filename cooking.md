---
layout: page
title: Cooking Articles
permalink: /cooking/
---

<p>I love cooking. Here are some of my receipes mostly for my reference.</p>
<ul>
  {% for item in site.cooking %}
    <li><a href="{{ item.url }}">{{ item.title }}</a></li>
  {% endfor %}
</ul> 
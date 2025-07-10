---
layout: page
title: Just Math
permalink: /just-math/
---

<ul>
  {% for item in site.just_math %}
    <li><a href="{{ item.url }}">{{ item.title }}</a></li>
  {% endfor %}
</ul> 
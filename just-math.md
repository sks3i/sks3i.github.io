---
layout: page
title: Just Math
permalink: /just-math/
---

<h2>Just Math Articles</h2>
<ul>
  {% for item in site.just_math %}
    <li><a href="{{ item.url }}">{{ item.title }}</a></li>
  {% endfor %}
</ul> 
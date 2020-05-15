---
layout: page
title: Links
description: 没有链接的博客是孤独的-
keywords: 友情链接
comments: true
menu: 链接
permalink: /links/
---

> God made relatives. Thank God we can choose our friends.

<ul>
{% for link in site.data.links %}
  {% if link.src == 'blog' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

> 😎 Awesome lists about all kinds of interesting topics

<ul>
{% for link in site.data.links %}
  {% if link.src == 'awesome' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>

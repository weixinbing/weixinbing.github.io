---
layout: page
title: About
description: 仰慕「优雅编码的艺术」，坚信熟能生巧，努力改变人生。
keywords: Xinbing Wei, 魏心兵
comments: true
menu: 关于
permalink: /about/
---

```
如果你没有找到一个当你睡觉时, 还在挣钱的方法, 你将一直工作到死！

                                ---- 沃伦 . 巴菲特
```

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
{% if site.url contains 'mazhuang.org' %}
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ site.url }}/assets/images/qrcode.jpg" alt="进击的程序员" />
</li>
{% endif %}
</ul>

## Skill Keywords

{% for category in site.data.skills %}

### {{ category.name }}

<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}

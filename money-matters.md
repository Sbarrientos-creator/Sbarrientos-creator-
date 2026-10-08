---
layout: page
title: Money Matters
permalink: /money-matters/
---

Investing basics, saving tips and news explained in simple language.
*Educational content only, not financial advice.*

{% for post in site.categories.money %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%b %d, %Y" }} by {{ post.author }}
{% endfor %}

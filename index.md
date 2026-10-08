---
layout: page
title: Welcome to CodeCapital
---

**Learn to code. Understand money.** CodeCapital is a student-run website that teaches
beginner coding and basic financial literacy in simple, practical ways.

## 💻 Latest from Code Corner

{% for post in site.categories.code limit:3 %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%b %d, %Y" }}
{% endfor %}

[See all coding posts →]({{ '/code-corner/' | relative_url }})

## 💰 Latest from Money Matters

{% for post in site.categories.money limit:3 %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%b %d, %Y" }}
{% endfor %}

[See all money posts →]({{ '/money-matters/' | relative_url }})

## 🛠️ Try a tool

Test the [Compound Interest Calculator]({{ '/tools/compound-interest.html' | relative_url }})
and see how small savings can grow over time.

## 💡 Tip of the week

> Start small and be consistent: 20 minutes of coding practice a day beats a 5-hour session once a month.

---

*CodeCapital is for educational purposes only and does not provide financial advice.*

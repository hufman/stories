---
layout: default
---

I spend a lot of time on projects, and I like to show them off!

{% for page in site.pages %}
  {% if page.title %}* [{{ page.title }}]({{ page.url | prepend: site.baseurl }}){% endif %}
{% endfor %}

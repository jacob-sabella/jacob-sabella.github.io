---
layout: page
icon: fas fa-code
order: 1
---

Things I've built in public. All MIT-licensed unless noted otherwise. Sources on [GitHub](https://github.com/jacob-sabella).

{% for p in site.data.projects %}
## [{{ p.title }}]({{ p.url | relative_url }})
**{{ p.tag }}**
{{ p.summary | markdownify | remove: '<p>' | remove: '</p>' }}
{% endfor %}

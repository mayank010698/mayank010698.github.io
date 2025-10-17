---
layout: default
title: notes
permalink: /notes/
nav: true
---

<h2>Research Notes</h2>

<ul>
{% assign notes = site.notes | sort: "date" | reverse %}
{% for n in notes %}
  <li>
    <a href="{{ n.url | relative_url }}">{{ n.title }}</a>
    <span style="color:#888;">— {{ n.date | date: "%b %d, %Y" }}</span>
    {% if n.tags %}<em> ({{ n.tags | join: ", " }})</em>{% endif %}
  </li>
{% endfor %}
</ul>

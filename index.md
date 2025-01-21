---
layout: default
title: Página de índice
---

# Índice de Páginas

<ul>
{% for page in site.pages %}
  {% unless page.title == "Página de índice" %}
    <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a></li>
  {% endunless %}
{% endfor %}
</ul>

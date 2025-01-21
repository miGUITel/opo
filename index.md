---
layout: default
title: Página de índice
---

# Índice de Páginas

<ul>
{% for page in site.pages %}
  {% unless page.url contains ".xml" or page.url contains ".css" or page.url contains "robots.txt" %}
    {% unless page.title == "Página de índice" %}
      <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title | default: page.url }}</a></li>
    {% endunless %}
  {% endunless %}
{% endfor %}
</ul>

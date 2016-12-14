---
layout: default
title: odoo
description: projects
category: true
permalink: /portfolio/odoo/
---

<ul class="categories center">
    <li><a href="/portfolio/">all</a></li>
    {% for category in site.categories %}
    <li><a href="/portfolio/{{ category }}/">{{ category }}</a></li>
    {% endfor %}
</ul>

{% for project in site.portfolio %}

{% if project.category == 'odoo' %}

{% include projects.html %}

{% endif %}

{% endfor %}

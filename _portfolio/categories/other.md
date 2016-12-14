---
layout: default
title: other
description: projects
category: true
permalink: /portfolio/other/
---

<ul class="categories center">
    <li><a href="/portfolio/">all</a></li>
    {% for category in site.categories %}
    <li><a href="/portfolio/{{ category }}/">{{ category }}</a></li>
    {% endfor %}
</ul>

{% for project in site.portfolio %}

{% if project.category == 'other' %}

{% include projects.html %}

{% endif %}

{% endfor %}

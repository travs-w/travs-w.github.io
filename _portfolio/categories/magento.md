---
layout: default
title: magento
description: projects
category: true
permalink: /portfolio/magento/
---

<ul class="categories center">
    <li><a href="/portfolio/">all</a></li>
    {% for category in site.categories %}
    <li><a href="/portfolio/{{ category }}/">{{ category }}</a></li>
    {% endfor %}
</ul>

{% for project in site.portfolio %}

{% if project.category == 'magento' %}

{% include projects.html %}

{% endif %}

{% endfor %}

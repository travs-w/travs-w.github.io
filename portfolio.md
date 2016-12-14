---
layout: default
title: portfolio
description: (part of) my life's work
permalink: /portfolio/
---

<ul class="categories center">
    {% for category in site.categories %}
    <li><a href="{{ site.baseurl }}/portfolio/{{ category }}/">{{ category }}</a></li>
    {% endfor %}
</ul>

{% for project in site.portfolio %}

{% if project.category != true %}

{% include projects.html %}

{% endif %}

{% endfor %}

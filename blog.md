---
layout: default
title: blog
description: (some of) my innermost thoughts
permalink: /blog/
---

<!-- <ul class="categories center">
    {% for category in site.categories %}
    <li><a href="{{ site.baseurl }}/blog/{{ category }}/">{{ category }}</a></li>
    {% endfor %}
</ul> -->

<ul class="post-list">
    {% for post in site.posts %}
      <li>
        <h2><a class="post-title" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
        <p class="post-meta">{{ post.date | date: '%B %-d, %Y — %H:%M' }}</p>
        <p>{{ post.description }}</p>
        <br/>
        <hr/>
      </li>
    {% endfor %}
</ul>
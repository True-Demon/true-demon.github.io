---
layout: archive
title: archive
classes:
  - splash
  - dark-theme
permalink: /archive/
---
{% assign postsByYear = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}
<nav class="menu browse by-year text-center" aria-label="year">
  <strong aria-hidden="true">Jump to:</strong>
  {% for year in postsByYear %}
  <a href="#{{ year.name }}"><span class="visually-hidden">jump to posts from</span>{{ year.name }}</a>
  {% endfor %}
</nav>
{% for year in postsByYear %}
<h2 id="{{ year.name }}">{{ year.name }}</h2>
<ul aria-label="posts from {{ year.name }}">
  {% for post in year.items %}
  <li>
    <p><a href="{{ post.url }}"><strong>{{ post.title }}</strong></a> <br/>| <i>{{ post.date | date: '%B %d' }}</i></p>
  </li>
  {% endfor %}
</ul>
{% endfor %}

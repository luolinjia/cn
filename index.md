---
title: Posts
layout: page
---

<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator"><span class="listing-logo"></span>{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%m-%d" }}">{{ post.date | date:"%m-%d" }}</time>
    <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}" class="listing-item-a">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>


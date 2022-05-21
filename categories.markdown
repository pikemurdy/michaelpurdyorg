---
layout: page
permalink: /category/
title: Categories
---


<div id="archives">
{% for category in site.categories %}
  <ul class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}

    <li class="category-head" id="#{{ category_name | slugize }}">{{ category_name }}
    <ul>
    {% for post in site.categories[category_name] %}
      <li><a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a></li>
    {% endfor %}
    </ul>
    </li>
  </ul>
{% endfor %}
</div>
---
layout: page
title: Blog Archive
permalink: /archives/
---


<iframe src="https://duckduckgo.com/search.html?site=michaelpurdy.org&prefill=Search MichaelPurdy.org" style="overflow:hidden;margin:0;padding:0;width:100%;height:40px;" frameborder="0"></iframe>

<ol class="archive">

{% for post in site.posts  %}
    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

    {% if forloop.first %}
    <li class="archive-year"><h2 id="{{ this_year }}-ref"  class="head-archive-year">{{this_year}}</h2>
    <ol>
    {% endif %}

    <li class="archive-item">
      <a href="{{ post.url }}"><span>{{ post.title }}</span></a>
      {% include excerpt.html %}
      {% comment %}
      {% include postcats.html %}
      {% endcomment %}
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-alarm-fill" viewBox="0 0 16 16">
  <path d="M6 .5a.5.5 0 0 1 .5-.5h3a.5.5 0 0 1 0 1H9v1.07a7.001 7.001 0 0 1 3.274 12.474l.601.602a.5.5 0 0 1-.707.708l-.746-.746A6.97 6.97 0 0 1 8 16a6.97 6.97 0 0 1-3.422-.892l-.746.746a.5.5 0 0 1-.707-.708l.602-.602A7.001 7.001 0 0 1 7 2.07V1h-.5A.5.5 0 0 1 6 .5zm2.5 5a.5.5 0 0 0-1 0v3.362l-1.429 2.38a.5.5 0 1 0 .858.515l1.5-2.5A.5.5 0 0 0 8.5 9V5.5zM.86 5.387A2.5 2.5 0 1 1 4.387 1.86 8.035 8.035 0 0 0 .86 5.387zM11.613 1.86a2.5 2.5 0 1 1 3.527 3.527 8.035 8.035 0 0 0-3.527-3.527z"/>
</svg><time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_string}}</time> 
    </li>

    {% if forloop.last %}
    {% else %}
        {% if this_year != next_year %}
        </ol></li><li>
        <h2 id="{{ next_year }}-ref" class="head-archive-year">{{next_year}}</h2>
        <ol>
        {% endif %}
    {% endif %}
{% endfor %}

</ol>


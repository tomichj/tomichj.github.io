---
layout: default
title: Welcome
hero: /images/front_hero.jpg
---

<div class="w-70-l fl pr4">
  <ul class="list pa0">

    {% for post in site.posts  %}
      {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
      {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

      {% if forloop.first %}
      <h3 id="{{ this_year }}-ref">{{this_year}}</h3>
      {% endif %}

      <li class="mb4-l mb3">
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="ttu f7 b mr2 tracked moon-gray db-l dn">{{ post.date | date: date_format }}</span>
        <a class="f4" href="{{ post.url | relative_url }}">
          {{ post.title | escape }}
        </a>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>

      {% if this_year != next_year %}
      <h3 id="{{ next_year }}-ref">{{next_year}}</h3>
      {% endif %}
    {% endfor %}
    
  </ul>
</div>

{%- include sidebar.html -%}

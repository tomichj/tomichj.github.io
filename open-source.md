---
layout: minimal
title: Open Source
---
<!-- <i class="fal fa-user black-50"></i> -->
<section class="pa0 center cf border-box tc mw7">
  {% assign sorted = site.projects | sort: 'order' %}
  {% for entry in sorted %}
    {%- assign bg_color = entry.bg-accent | default: "bg-washed-red" -%}
    <article class="w-100 w-50-m w-33-l fl bg-white">
      <div class="br3 pa2 pa3-ns mv3 mr1-m mr1-l mh1 ba b--black-10 h6 relative">
        <div class="tc">
          <i class="fal {{ entry.fa-icon }} fa-2x black-50 mb2 ph5 pv3 br2"></i>
          <h1 class="f4 lh-title-ns mv0">
            {{ entry.title }}
          </h1>
          <hr class="w3 bb bw1 b--black-10 mv3 {{ bg_color }}">
        </div>
        <p class="lh-copy measure center f6 black-70 m0">
          {{ entry.description }}
        </p>
        <div class="absolute left-0 bottom-0 right-0 h3 ph3 pt3">
          <a href="{{ entry.github }}" class="dib tc w-100 m0 strong ttu f6 {{ bg_color }}">
            Github Repo
          </a>
        </div>
      </div>
    </article>
    
  {% endfor %}
</section>

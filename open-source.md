---
layout: minimal
title: Open Source
---

<section class="pa0 center cf border-box tc mw7">
  {% for entry in site.projects %}
    
    <article class="w-100 w-50-m w-33-l fl bg-white">
      <div class="br3 pa3 pa4-ns mv3 mh1 ba b--black-10">
        <div class="tc">
          <img src="{{ entry.img }}" class="br-100 h3 w3 dib" title="{{ entry.title }}">
          <h1 class="f4">{{ entry.title }}</h1>
          <hr class="w3 bb bw1 b--black-10 mv3">
        </div>
        <p class="lh-copy measure center f6 black-70 h3">
        {{ entry.description }}
        </p>
        <a href="{{ entry.github }}" class="tc dib w-80">
          <p class="lh-copy strong ttu f5 invert tc">Github Repo</p>
        </a>
      </div>
    </article>
    
  {% endfor %}
</section>

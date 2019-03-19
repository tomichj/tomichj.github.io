---
layout: minimal
title: Open Source
---

<div class="pa0">
  <ul class="list pa0 cf">

    {% for project in site.projects %}
      
      <article class="mw5 fl bg-white br3 pa3 pa4-ns mv3 mh1 ba b--black-10">
        <div class="tc">
          <img src="{{ project.img-src }}" class="br-100 h3 w3 dib" title="Photo of a kitty staring at you">
          <h1 class="f4">{{ project.title }}</h1>
          <hr class="mw3 bb bw1 b--black-10">
        </div>
        <p class="lh-copy measure center f6 black-70">
          {{ project.content }}
        </p>
      </article>
      
    {% endfor %}

  </ul>
</div>

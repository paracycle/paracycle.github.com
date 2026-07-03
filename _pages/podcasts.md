---
title: Podcasts
layout: default
permalink: /podcasts/
published: true
---

<section class="def-block">
  <div class="section-head">
    <h2><span class="kw">def</span>podcasts</h2>
  </div>
  <div class="section-body">
    {% assign podcasts = site.podcasts | sort: 'date' | reverse %}
    {% assign prev_year = "" %}
    {% for podcast in podcasts %}
    {% capture this_year %}{{ podcast.date | date: '%Y' }}{% endcapture %}
    {% if this_year != prev_year %}
    <p class="list-marker">{{ this_year }}</p>
    {% endif %}
    <article class="entry" id="{{ podcast.title | slugify }}">
      <h3 class="entry-title">{{ podcast.title }}</h3>
      <p class="entry-meta">
        {{ podcast.podcast }}
        {% if podcast.episode %} · episode {{ podcast.episode }}{% endif %}
        · <time datetime="{{ podcast.date | date_to_xmlschema }}">{{ podcast.date | date: '%B %Y' }}</time>
      </p>
      <details class="entry-abstract">
        <summary>show notes</summary>
        <div class="entry-content">{{ podcast.content }}</div>
      </details>
      {% if podcast.link or podcast.video %}
      <p class="entry-links">
        {% if podcast.link %}<a href="{{ podcast.link }}">listen</a>{% endif %}
        {% if podcast.video %}<a href="{{ podcast.video }}">watch</a>{% endif %}
      </p>
      {% endif %}
    </article>
    {% capture prev_year %}{{ this_year }}{% endcapture %}
    {% endfor %}
  </div>
  <p class="section-end">end</p>
</section>

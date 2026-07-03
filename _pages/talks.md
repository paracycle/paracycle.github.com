---
title: Talks
layout: default
permalink: /talks/
published: true
---

<section class="def-block">
  <div class="section-head">
    <h2><span class="kw">def</span>talks</h2>
  </div>
  <div class="section-body">
    {% assign talks = site.talks | sort: 'date' | reverse %}
    {% assign prev_year = "" %}
    {% for talk in talks %}
    {% capture this_year %}{{ talk.date | date: '%Y' }}{% endcapture %}
    {% if this_year != prev_year %}
    <p class="list-marker">{{ this_year }}</p>
    {% endif %}
    <article class="entry" id="{{ talk.title | slugify }}">
      <h3 class="entry-title">{{ talk.title }}</h3>
      <p class="entry-meta">{{ talk.event }} · <time datetime="{{ talk.date | date_to_xmlschema }}">{{ talk.date | date: '%B %Y' }}</time></p>
      <details class="entry-abstract">
        <summary>abstract</summary>
        <div class="entry-content">{{ talk.content }}</div>
      </details>
      {% if talk.video or talk.slides or talk.site %}
      <p class="entry-links">
        {% if talk.video %}<a href="{{ talk.video }}">video</a>{% endif %}
        {% if talk.slides %}<a href="{{ talk.slides }}">slides</a>{% endif %}
        {% if talk.site %}<a href="{{ talk.site }}">site</a>{% endif %}
      </p>
      {% endif %}
    </article>
    {% capture prev_year %}{{ this_year }}{% endcapture %}
    {% endfor %}
  </div>
  <p class="section-end">end</p>
</section>

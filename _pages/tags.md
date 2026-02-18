---
title: "Posts by Tag"
layout: tags
permalink: /tags/
author_profile: true
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: https://www.andrewkapaldo.com/assets/images/hero-banner.jpg
---

Browse all writeups organized by challenge type and other tags.

{% assign tags_sorted = site.tags | sort %}

{% for tag in tags_sorted %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}

  <h2 id="{{ t | slugify }}" class="archive__subtitle">{{ t }}</h2>
  
  <div class="entries-{{ page.entries_layout | default: 'list' }}">
    {% for post in posts %}
      {% include archive-single.html %}
    {% endfor %}
  </div>
  <a href="#page-title" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
{% endfor %}

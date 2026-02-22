---
title: "Posts by Category"
layout: archive
permalink: /categories/
author_profile: true
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: https://www.andrewkapaldo.com/assets/images/hero-banner.jpg
---

Browse all writeups organized by competition platform.

<style>
.category-section {
  margin-bottom: 3em;
}

.category-section h2 {
  border-bottom: 2px solid #1f6feb;
  padding-bottom: 0.3em;
  margin-bottom: 1em;
}

.category-count {
  font-size: 0.75em;
  color: #8b949e;
  font-weight: normal;
  margin-left: 0.5em;
}

.category-section,
.tag-section {
  clear: both;
  margin-bottom: 3em;
}
</style>

<div class="jump-pills">
{% assign categories_sorted = site.categories | sort %}
{% for category in categories_sorted %}
  <a href="#{{ category[0] | downcase | replace: ' ', '-' }}" class="jump-pill">
    {{ category[0] | replace: '-', ' ' }}<span class="jump-pill__count">{{ category[1] | size }}</span>
  </a>
{% endfor %}
</div>

{% assign categories_sorted = site.categories | sort %}
{% for category in categories_sorted %}
<div class="category-section" id="{{ category[0] | downcase | replace: ' ', '-' }}">
  <h2>{{ category[0] | replace: '-', ' ' }}<span class="category-count">({{ category[1] | size }})</span></h2>
  <div class="grid__wrapper">
    {% for post in category[1] %}
      {% include archive-single.html type="grid" %}
    {% endfor %}
  </div>
</div>
{% endfor %}

---
title: "Posts by Tag"
layout: archive
permalink: /tags/
author_profile: true
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: https://www.andrewkapaldo.com/assets/images/hero-banner.jpg
---

Browse all writeups organized by challenge type and other tags.

<style>
.tag-section {
  margin-bottom: 3em;
}

.tag-section h2 {
  border-bottom: 2px solid #1f6feb;
  padding-bottom: 0.3em;
  margin-bottom: 1em;
}

.tag-count {
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


<div class="category-jump">
{% assign tags_sorted = site.tags | sort %}
{% for tag in tags_sorted %}
  <a href="#{{ tag[0] | downcase | replace: ' ', '-' }}" class="filter-btn">{{ tag[0] }} <span>({{ tag[1] | size }})</span></a>
{% endfor %}
</div>

{% assign tags_sorted = site.tags | sort %}
{% for tag in tags_sorted %}
<div class="tag-section" id="{{ tag[0] | downcase | replace: ' ', '-' }}">
  <h2>{{ tag[0] }}<span class="tag-count">({{ tag[1] | size }})</span></h2>
  <div class="grid__wrapper">
    {% for post in tag[1] %}
      {% include archive-single.html type="grid" %}
    {% endfor %}
  </div>
</div>
{% endfor %}

---
layout: archive
title: "CTF Writeups"
permalink: /writeups/
author_profile: false
classes: wide
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: https://www.andrewkapaldo.com/assets/images/hero-banner.jpg
excerpt: "Documented solutions and learning from CTF competitions"
---

<style>
.writeup-filters {
  margin: 2em 0;
  padding: 1.5em;
  background: rgba(255,255,255,0.05);
  border-radius: 8px;
}

.filter-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5em;
  margin-top: 1em;
}

.filter-btn {
  padding: 0.5em 1em;
  background: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

.filter-btn:hover {
  background: rgba(255,255,255,0.2);
  transform: translateY(-2px);
}

.writeup-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1em;
  margin: 2em 0;
}

.stat-card {
  padding: 1.5em;
  background: linear-gradient(135deg, rgba(52,152,219,0.1) 0%, rgba(155,89,182,0.1) 100%);
  border-radius: 8px;
  text-align: center;
  border: 1px solid rgba(255,255,255,0.1);
}

.stat-number {
  font-size: 2.5em;
  font-weight: bold;
  color: #3498db;
}

.stat-label {
  font-size: 0.9em;
  color: rgba(255,255,255,0.7);
  text-transform: uppercase;
  letter-spacing: 1px;
}
</style>

## 📊 Statistics

<div class="writeup-stats">
  <div class="stat-card">
    <div class="stat-number">{{ site.posts | size }}</div>
    <div class="stat-label">Total Writeups</div>
  </div>
  <div class="stat-card">
    <div class="stat-number">{{ site.categories | size }}</div>
    <div class="stat-label">Competitions</div>
  </div>
  <div class="stat-card">
    <div class="stat-number">{{ site.tags | size }}</div>
    <div class="stat-label">Challenge Types</div>
  </div>
</div>

---

## 🔍 Filter by Competition

<div class="writeup-filters">
  <p><strong>Quick Navigation:</strong></p>
  <div class="filter-buttons">
    <a href="#huntress" class="filter-btn">🦌 Huntress CTF</a>
    <a href="#flare-on" class="filter-btn">🔥 Flare-On</a>
    <a href="#flare-io" class="filter-btn">💻 Flare.io</a>
    <a href="#hackthebox" class="filter-btn">📦 HackTheBox</a>
    <a href="#tryhackme" class="filter-btn">🎯 TryHackMe</a>
    <a href="#picoctf" class="filter-btn">🏴‍☠️ PicoCTF</a>
    <a href="/categories/" class="filter-btn">📂 All Categories</a>
    <a href="/tags/" class="filter-btn">🏷️ All Tags</a>
  </div>
</div>

---

## 🦌 Huntress CTF {#huntress}

Huntress CTF is an annual cybersecurity competition focused on realistic threat hunting and incident response scenarios.

{% assign huntress_posts = site.categories.Huntress %}
{% if huntress_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in huntress_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#huntress" class="btn btn--primary">View All Huntress Writeups ({{ huntress_posts.size }}) →</a>
</div>
{% else %}
<p><em>No Huntress writeups yet. Check back soon!</em></p>
{% endif %}

---


## 💻 Flare.io {#flare-io}

Flare.io offers modern security challenges and training for real-world vulnerabilities.

{% assign flareio_posts = site.categories.Flare-io %}
{% if flareio_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in flareio_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#flare-io" class="btn btn--primary">View All Flare.io Writeups ({{ flareio_posts.size }}) →</a>
</div>
{% else %}
<p><em>No Flare.io writeups yet. Check back soon!</em></p>
{% endif %}

---
{% comment %}
## 🔥 Flare-On {#flare-on}

Flare-On is Mandiant's annual reverse engineering challenge, featuring increasingly difficult malware analysis tasks.

{% assign flareon_posts = site.categories.Flare-On %}
{% if flareon_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in flareon_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#flare-on" class="btn btn--primary">View All Flare-On Writeups ({{ flareon_posts.size }}) →</a>
</div>
{% else %}
<p><em>No Flare-On writeups yet. Check back soon!</em></p>
{% endif %}

---
{% endcomment %}

## 🎯 TryHackMe {#tryhackme}

TryHackMe provides guided learning paths and CTF-style rooms for all skill levels.

{% assign thm_posts = site.categories.THM %}
{% if thm_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in thm_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#thm" class="btn btn--primary">View All THM Writeups ({{ thm_posts.size }}) →</a>
</div>
{% else %}
<p><em>No TryHackMe writeups yet. Check back soon!</em></p>
{% endif %}

---
{% comment %}
## 📦 HackTheBox {#hackthebox}

HackTheBox offers realistic penetration testing labs with vulnerable machines and challenges.

{% assign htb_posts = site.categories.HackTheBox %}
{% if htb_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in htb_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#hackthebox" class="btn btn--primary">View All HTB Writeups ({{ htb_posts.size }}) →</a>
  </div>
{% else %}
<p><em>No HackTheBox writeups yet. Check back soon!</em></p>
{% endif %}

---

## 🏴‍☠️ PicoCTF {#picoctf}

PicoCTF is a free computer security education program with a capture-the-flag style competition, primarily for middle and high school students but open to all.

{% assign pico_posts = site.categories.PicoCTF %}
{% if pico_posts.size > 0 %}
<div class="grid__wrapper">
  {% for post in pico_posts limit:20 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
<div style="clear: both; text-align: center; margin-top: 2em;">
  <a href="/categories/#picoctf" class="btn btn--primary">View All PicoCTF Writeups ({{ pico_posts.size }}) →</a>
</div>
{% else %}
<p><em>No PicoCTF writeups yet. Check back soon!</em></p>
{% endif %}

---
{% endcomment %}

## 📚 All Writeups (Chronological)

<div class="search-container">
  <input
    type="text"
    id="writeup-search"
    placeholder="🔍 Search by title, tag, or competition..."
    autocomplete="off"
  />
</div>
<div class="year-filter">
  <span class="year-filter__label">Filter by year:</span>
  <span class="year-pill active" data-year="all">All Years</span>
  <span class="year-pill" data-year="2025">2025</span>
  <span class="year-pill" data-year="2026">2026</span>
</div>
<div id="search-meta"></div>
<div id="search-results">
  {% for post in site.posts %}
  <div class="search-item"
    data-title="{{ post.title | downcase }}"
    data-tags="{{ post.tags | join: ' ' | downcase }}"
    data-category="{{ post.categories | first | downcase }}"
    data-year="{{ post.competition_year }}">
    {% include archive-single.html %}
  </div>
  {% endfor %}
</div>
<script>
const input = document.getElementById('writeup-search');
const meta  = document.getElementById('search-meta');
const items = document.querySelectorAll('.search-item');
const yearPills = document.querySelectorAll('.year-pill');

let activeYear = 'all';

function filterItems() {
  const query = input.value.trim().toLowerCase();
  let visible = 0;

  items.forEach(item => {
    const matchesSearch = !query
      || item.dataset.title.includes(query)
      || item.dataset.tags.includes(query)
      || item.dataset.category.includes(query);

    const matchesYear = activeYear === 'all' || item.dataset.year === activeYear;

    const matches = matchesSearch && matchesYear;
    item.classList.toggle('search-item--hidden', !matches);
    if (matches) visible++;
  });

  const total = items.length;
  if (query || activeYear !== 'all') {
    meta.textContent = visible === 0
      ? 'No writeups found.'
      : `Showing ${visible} of ${total} writeups`;
  } else {
    meta.textContent = '';
  }
}

input.addEventListener('input', filterItems);

yearPills.forEach(pill => {
  pill.addEventListener('click', function() {
    yearPills.forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    activeYear = this.dataset.year;
    filterItems();
  });
});
</script>

---

## 🏷️ Browse by Category

Prefer to browse by challenge type? Check out these popular categories:

- [🌐 Web](/tags/#web) - SQL injection, XSS, authentication bypasses
- [⚒️ Binary Exploitation](/tags/#binary) - Buffer overflows, ROP chains, shellcode
- [🔐 Crypto](/tags/#crypto) - Classical ciphers, modern cryptanalysis
- [🔍 Forensics](/tags/#forensics) - Memory dumps, disk analysis, network captures
- [🐞 Malware](/tags/#malware) - Malware analysis, reverse engineering
- [🕵️ OSINT](/tags/#osint) - Open source intelligence gathering
- [🤖 AI](/tags/#ai) - Artificial Intelligence
- [📦 Miscellaneous](/tags/#misc) - Unique challenges that don't fit other categories

<p style="text-align: center; margin-top: 2em;"><a href="/tags/" class="btn btn--info btn--large">Browse All Tags →</a></p>

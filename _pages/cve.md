---
layout: archive
title: "CVE Research"
permalink: /cve/
author_profile: false
classes: wide
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: /assets/images/hero-banner.jpg
excerpt: "Proof of concept research and technical analysis of CVEs"
---

<style>
.cve-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  gap: 1.4em;
  margin: 1.5em 0;
}

.cve-card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px;
  padding: 1.4em;
  display: flex;
  flex-direction: column;
  gap: 0.7em;
  transition: border-color 0.2s, transform 0.2s;
  text-decoration: none;
  color: inherit;
}

.cve-card:hover {
  border-color: #1f6feb;
  transform: translateY(-2px);
  color: inherit;
  text-decoration: none;
}

.cve-card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.5em;
}

.cve-card__id {
  font-family: 'Share Tech Mono', monospace;
  font-size: 1em;
  font-weight: 700;
  color: #58a6ff;
}

.cve-card__title {
  font-size: 0.95em;
  font-weight: 600;
  color: #e6edf3;
  line-height: 1.4;
}

.cve-card__desc {
  font-size: 0.85em;
  color: #8b949e;
  line-height: 1.6;
}

.cve-card__meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4em;
  margin-top: 0.2em;
}

.cve-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.3em;
  font-size: 0.72em;
  padding: 3px 8px;
  border-radius: 20px;
  border: 1px solid;
  font-weight: 500;
}

.badge-critical { color: #ff6b6b; border-color: rgba(255,107,107,0.4); background: rgba(255,107,107,0.1); }
.badge-high     { color: #f0a500; border-color: rgba(240,165,0,0.4);   background: rgba(240,165,0,0.1); }
.badge-medium   { color: #e3b341; border-color: rgba(227,179,65,0.4);  background: rgba(227,179,65,0.1); }
.badge-low      { color: #3fb950; border-color: rgba(63,185,80,0.4);   background: rgba(63,185,80,0.1); }

.badge-patched   { color: #3fb950; border-color: rgba(63,185,80,0.4);   background: rgba(63,185,80,0.08); }
.badge-unpatched { color: #ff6b6b; border-color: rgba(255,107,107,0.4); background: rgba(255,107,107,0.08); }

.badge-vendor  { color: #8b949e; border-color: rgba(139,148,158,0.3); background: rgba(139,148,158,0.08); }
.badge-cvss    { color: #bc8cff; border-color: rgba(188,140,255,0.4); background: rgba(188,140,255,0.08); }
.badge-poc     { color: #58a6ff; border-color: rgba(88,166,255,0.4);  background: rgba(88,166,255,0.08); }

.cve-card__date {
  font-size: 0.75em;
  color: #8b949e;
}

.cve-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 1em;
  margin: 1.5em 0 2em;
}

.cve-stat-card {
  padding: 1.2em;
  background: linear-gradient(135deg, rgba(218,54,51,0.1) 0%, rgba(188,140,255,0.1) 100%);
  border-radius: 8px;
  text-align: center;
  border: 1px solid rgba(255,255,255,0.1);
}

.cve-stat-number {
  font-size: 2em;
  font-weight: bold;
  color: #da3633;
}

.cve-stat-label {
  font-size: 0.8em;
  color: rgba(255,255,255,0.6);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.no-cve {
  text-align: center;
  padding: 3em;
  color: #8b949e;
  font-size: 0.95em;
}

/* Severity filter pills */
.severity-filter {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.5em;
  margin: 1em 0 1.5em;
}

.severity-filter__label {
  font-size: 0.85em;
  color: #8b949e;
}

.severity-pill {
  display: inline-flex;
  align-items: center;
  padding: 0.3em 0.9em;
  border-radius: 20px;
  border: 1px solid rgba(255,255,255,0.15);
  background: rgba(255,255,255,0.05);
  color: #8b949e;
  font-size: 0.8em;
  cursor: pointer;
  transition: all 0.15s;
  user-select: none;
}

.severity-pill:hover,
.severity-pill.active {
  border-color: rgba(31,111,235,0.8);
  background: rgba(31,111,235,0.15);
  color: #58a6ff;
  font-weight: 600;
}
</style>

## 📊 Overview

<div class="cve-stats">
  <div class="cve-stat-card">
    <div class="cve-stat-number">{{ site.cve | size }}</div>
    <div class="cve-stat-label">CVEs Researched</div>
  </div>
  <div class="cve-stat-card">
    <div class="cve-stat-number">{{ site.cve | where_exp: "p", "p.severity == 'Critical'" | size }}</div>
    <div class="cve-stat-label">Critical</div>
  </div>
  <div class="cve-stat-card">
    <div class="cve-stat-number">{{ site.cve | where_exp: "p", "p.severity == 'High'" | size }}</div>
    <div class="cve-stat-label">High</div>
  </div>
  <div class="cve-stat-card">
    <div class="cve-stat-number">{{ site.cve | where_exp: "p", "p.patch_status == 'Unpatched'" | size }}</div>
    <div class="cve-stat-label">Unpatched</div>
  </div>
</div>

---

## 🔍 CVE Research

<div class="severity-filter">
  <span class="severity-filter__label">Filter by severity:</span>
  <span class="severity-pill active" data-severity="all">All</span>
  <span class="severity-pill" data-severity="critical">Critical</span>
  <span class="severity-pill" data-severity="high">High</span>
  <span class="severity-pill" data-severity="medium">Medium</span>
  <span class="severity-pill" data-severity="low">Low</span>
</div>

<div class="cve-grid" id="cve-grid">
  {% assign cve_posts = site.cve | sort: 'date' | reverse %}
  {% if cve_posts.size > 0 %}
    {% for post in cve_posts %}
    <a href="{{ post.url }}" class="cve-card" data-severity="{{ post.severity | downcase }}">
      <div class="cve-card__header">
        <span class="cve-card__id">{{ post.cve_id }}</span>
        <span class="cve-card__date">{{ post.date | date: "%b %d, %Y" }}</span>
      </div>
      <div class="cve-card__title">{{ post.title }}</div>
      <div class="cve-card__desc">{{ post.excerpt | strip_html | truncate: 120 }}</div>
      <div class="cve-card__meta">
        {% if post.severity %}
          <span class="cve-badge badge-{{ post.severity | downcase }}">
            <i class="fas fa-circle"></i> {{ post.severity }}
          </span>
        {% endif %}
        {% if post.cvss %}
          <span class="cve-badge badge-cvss">CVSS {{ post.cvss }}</span>
        {% endif %}
        {% if post.vendor %}
          <span class="cve-badge badge-vendor"><i class="fas fa-building"></i> {{ post.vendor }}</span>
        {% endif %}
        {% if post.patch_status %}
          <span class="cve-badge badge-{{ post.patch_status | downcase }}">
            {% if post.patch_status == "Patched" %}<i class="fas fa-check"></i>{% else %}<i class="fas fa-exclamation-triangle"></i>{% endif %}
            {{ post.patch_status }}
          </span>
        {% endif %}
        {% if post.has_poc %}
          <span class="cve-badge badge-poc"><i class="fas fa-code"></i> PoC</span>
        {% endif %}
      </div>
    </a>
    {% endfor %}
  {% else %}
    <div class="no-cve">
      <i class="fas fa-shield-alt" style="font-size:2em; margin-bottom:0.5em; display:block;"></i>
      No CVE research published yet. Check back soon!
    </div>
  {% endif %}
</div>

<script>
document.querySelectorAll('.severity-pill').forEach(function(pill) {
  pill.addEventListener('click', function() {
    document.querySelectorAll('.severity-pill').forEach(p => p.classList.remove('active'));
    this.classList.add('active');
    const severity = this.dataset.severity;
    document.querySelectorAll('.cve-card').forEach(function(card) {
      if (severity === 'all' || card.dataset.severity === severity) {
        card.style.display = '';
      } else {
        card.style.display = 'none';
      }
    });
  });
});
</script>

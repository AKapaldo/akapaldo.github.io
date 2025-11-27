---
layout: default
title: Home
---

<!-- Hero Section -->
<div class="p-5 mb-4 bg-light rounded-3 text-center">
  <div class="container">
    <h1 class="display-4">Hi, I'm Andrew</h1>
    <p class="lead">
      I'm a technology enthusiast passionate about cybersecurity, Capture the Flag (CTF) challenges, programming, and smart home automation.
      I live in Morgantown, WV, volunteer with the local search and rescue team, and hold several certifications including CompTIA A+, Network+, Security+, (ISC)² SSCP & CCSP.
      I'm currently pursuing my MS in Business Cybersecurity Management, expected Summer 2026.
    </p>
  </div>
</div>

<!-- Latest CTF Posts -->
<div class="container my-5">
  <h2 class="mb-4">Latest CTF Writeups</h2>
  <div class="row row-cols-1 row-cols-md-2 g-4">
    {% assign ctf_posts = site.posts | where:"category","ctf" | sort: "date" | reverse %}
    {% for post in ctf_posts limit:4 %}
      <div class="col">
        <div class="card h-100 shadow-sm">
          <div class="card-body">
            <h5 class="card-title">{{ post.title }}</h5>
            <p class="card-text">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
            <a href="{{ post.url | relative_url }}" class="btn btn-primary">Read More</a>
          </div>
          <div class="card-footer text-muted">
            {{ post.date | date: "%b %-d, %Y" }}
          </div>
        </div>
      </div>
    {% endfor %}
  </div>
</div>

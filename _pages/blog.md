---
layout: default
title: CTF Writeups
---

<div class="container mt-5">
  <h1 class="display-3 text-center">CTF Writeups</h1>

  <div class="row">
    {% for post in site.posts %}
      <div class="col-md-6 mb-4">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">{{ post.title }}</h5>
            <p class="card-text text-muted">
              Competition: {{ post.competition }}<br>
              Challenge: {{ post.challenge }}<br>
              Date: {{ post.date | date: "%B %-d, %Y" }}
            </p>
            <a href="{{ post.url }}" class="btn btn-primary">Read Writeup</a>
          </div>
        </div>
      </div>
    {% endfor %}
  </div>
</div>

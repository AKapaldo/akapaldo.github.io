---
layout: home
title: "Hi, I'm Andrew"
author_profile: true
---

## About Me

Hey, I'm Andrew — a cybersecurity geek who loves solving problems and making technology work better for everyone.

I specialize in:
- Cybersecurity operations and analysis  
- Software supply chain risks  
- Capture the Flag (CTF) competitions  
- Automation and scripting  
- Smart home & IoT technologies  
- Search and Rescue field operations  

I live in Morgantown, WV, work in IT, volunteer on a SAR team, and I'm currently pursuing my **MS in Business Cybersecurity Management** (expected Summer 2026).

I also hold:
- CompTIA A+, Network+, Security+  
- (ISC)² SSCP and CCSP  
- BS in Cybersecurity & Information Assurance  
- AAS in Information Systems  
- Certificate in Applied Cybersecurity  

---

## Latest Blog Posts

Below are my 5 newest posts, including automatically synced CTF writeups:

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url | relative_url }})  
  <small>{{ post.date | date: "%B %-d, %Y" }}</small>
{% endfor %}

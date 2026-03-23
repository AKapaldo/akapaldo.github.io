---
title: "PingCMD"
date: 2026-03-22
categories:
  - PicoCTF
tags:
  - "Miscellaneous"
  - "2026"
  - "Command Injection"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 📦 ping-cmd

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous         |Yahaya Meddy        |

## Challenge Prompt

Can you make the server reveal its secrets? It seems to be able to ping Google DNS, but what happens if you get a little creative with your input?

## Problem Type
- Command Injection


## Solve
When we `nc` into the machine we are prompted to `Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'):`

If we enter `8.8.8.8` the machine will ping the IP, but anything else we enter like `8.8.4.4` fails.
<img width="792" height="376" alt="2026-03-22_17-59-17" src="https://github.com/user-attachments/assets/854a4827-4d56-4d67-96cf-a19632de170e" />

But what if we add an extra command onto the end of our allowed IP with `;`?<br>
If we connect again and this time send `8.8.8.8; cat flag.txt`, the machine will ping the allowed IP and then run the `cat` command and output our flag:
<img width="905" height="241" alt="2026-03-22_18-00-48" src="https://github.com/user-attachments/assets/eec2d47c-6b37-4622-95f4-632b7289923c" />


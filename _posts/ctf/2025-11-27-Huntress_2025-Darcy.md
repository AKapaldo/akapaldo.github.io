---
title: "Huntress 2025 – Darcy"
layout: post
competition: Huntress 2025
challenge: Darcy
tags: 
  - Huntress
  - 2025
  - Forensics
---

# 🔍 Darcy

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics          |John Hammond         |

## Challenge Prompt

Darcy has apparently been having a lot of fun with a unique version control system.

She told me she hid a flag somewhere with her new tool and wants me to find it... I can't make any sense of it, can you?

## Problem Type
- Forensics

## Solve
Extract folder and open

```bash
cat * | grep -iEr "flag{"
```


## Flag
`flag{a0c1e852e1281d134f0ac2b8615183a3}`


<p align="right">(<a href="#top">back to top</a>)</p>

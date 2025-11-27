---
title: "Huntress 2025 – Beyblade"
layout: post
competition: Huntress 2025
challenge: Beyblade
tags: 
  - Huntress
  - 2025
  - Forensics
---

# 🔍 Beyblade

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics     |John Hammond           |

## Challenge Prompt

Sheesh! Some threat actor sure did let it rip on this host! We've been able to uncover a file that may help with incident response.

> [!NOTE]
> 1. The password to the ZIP archive is `beyblade`.
> 2. This challenge has the flag MD5 hash value separated into chunks. You must uncover all of the different pieces and put them together with the flag{ and } suffix to submit.

## Problem Type
- Registry Forensics

## Solve
	1. 47cb (run)
	2. 5cd4 (run)
	3. 6d7b (typedURLs)
	4. B34a (fileless)
	5. 0d9c (typedPaths)
	6. 315a (apppaths)
	7. 99bb (muicache)
	8. 58de (tscclient_tln)

All commands are:

```bash
Regripper -r beyblade -p <plugin>
```

## Flag
`flag{47cb5cd46d7bb34a0d9c315a99bb58de}`


<p align="right">(<a href="#top">back to top</a>)</p>

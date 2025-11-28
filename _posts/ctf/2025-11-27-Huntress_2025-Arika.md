---
title: "Arika"
date: 2025-11-27
categories:
  - Huntress
tags:
  - Huntress
  - "2025"
  - Web
  - Easy
difficulty: Easy
platform: Huntress CTF
competition_year: "2025"
toc: true
toc_sticky: true
---


# 🌐 ARIKA

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web           |John Hammond           |

## Challenge Prompt

The Arika ransomware group likes to look slick and spiffy with their cool green-on-black terminal style website... 
but it sounds like they are worried about some security concerns of their own!

## Password
> [!NOTE]
> The password for the ZIP archive below is `arika`.

## Problem Type
- Web command injection

## Solve
Press F12

Run a command like `whoami`

Open network tab

Right click to edit and resend

Send `{"command":"whoami\ncat flag.txt"}`

Open response tab


## Flag
`flag{eaec346846596f7976da7e1adb1f326d}`


<p align="right">(<a href="#top">back to top</a>)</p>

---
title: "Spaghetti"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Malware"
  - "2025"
  - "PowerShell"
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 🐞 Spaghetti

|Category         |	Author                |
|-----------------|-----------------------|
|🐞 Malware       |John Hammond           |

## Challenge Prompt

You know, I've been thinking... at the end of the day, spaghetti is really just strings of pasta!

Anyway, we saw this weird file running on startup. Can you figure out what this is?

I'm sure you'll get more understanding of the questions below as you explore!

> [!CAUTION]
> This is the Malware category, and as such, includes malware. Please be sure to analyze these files within an isolated virtual machine.

> [!NOTE]
> You may find a public paste URL that is expired. This is an artifact of the original malware sample and is intentional.
> This URL is not necessary for the challenge.

## Password
> [!IMPORTANT]
> The ZIP archive password is `infected`.

## Problem Type
- PowerShell

## Solve 1
Line 1500 has the filename (AYGIW.tmp)
Line 3228 has that it is bytes and to replace WT with 00
Paste contents of AYGIW to Cyber Chef
* Find/Replace WT (simple string) with `00`
* From Hex
* Remove Null Bytes
Beginning of extract is gibberish but part way down is the flag.

## Flag 1
`flag{39544d3b5374ebf7d39b8c260fc4afd8}`

## Solve 2
Line 3501 "MyOasis4" - copy `~%` blob
Paste to Cyber Chef
* Find/Replace ~ (simple string) with `0`
* Find/Replace % (simple string) with `1`
* From Binary

Grab line with HtMldECoDE
Paste to Cyber Chef
* From HTML Entity

## Flag 2
`flag{b313794dcef335da6206d54af81b6203}`

## Solve 3
Line 3503 "Tdefo" - copy `~%` blob
Paste to Cyber Chef
* Find/Replace ~ (simple string) with `0` 
* Find/Replace % (simple string) with `1`
* From Binary

## Flag 3
`flag{60814731f508781b9a5f8636c817af9d}`


<p align="right">(<a href="#top">back to top</a>)</p>

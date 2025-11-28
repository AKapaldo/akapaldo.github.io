---
title: "Tabbys Date"
date: 2025-11-28
categories:
  - Huntress_2025
tags:
  - CTF
platform: Huntress_2025
toc: true
toc_sticky: true
---

# 🔍 Tabby's Date

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics     |John Hammond           |

## Challenge Prompt

Ohhhh, Tab, Tab, Tab.... what has she done.

My friend Tabby just got a new laptop and she's been using it to take notes. She says she puts her whole life on there!

She was so excited to finally have a date with a boy she liked, but she completely forgot the details of where and when. She told me she remembers writing it in a note... but she doesn't think she saved it!!

She shared with us an export of her laptop files.

## Password
> [!IMPORTANT]
> The password to the ZIP archive is `tabbys_date`.

## Problem Type
- Windows Notepad Files

## Solve

List out files in the structure
```bash
ls -lR
```
Found `\users\tabby\AppData\Local\Packages\Microsoft.WindowsNotepad_8wekyb3d8bbwe\LocalState\`<br>
.bin files located here are notepad files
<br>
```bash
hexdump -C *
```
Dump hex and ASCII and then did a find for "{"

## Flag

`flag{165d19b610c02b283fc1a6b4a54c4a58}`


<p align="right">(<a href="#top">back to top</a>)</p>

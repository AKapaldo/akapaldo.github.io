---
title: "For Greatness"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Malware"
  - "2025"
  - "PHP Malware"
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 🐞 For Greatness

|Category         |	Author                |
|-----------------|-----------------------|
|🐞 Malware       |John Hammond           |

## Challenge Prompt

Oh great, another phishing kit. This has some functionality to even send stolen data over email! 

Can you track down the email address they send things to?

This is the `Malware` category, and as such, includes malware.
Please be sure to analyze these files within an isolated virtual machine.
{: .notice--warning}


## Problem Type
- PHP Malware

## Solve
Extract file and open `j.PHP`

Inside there is a line that starts `$FRczk`
Put that into CyberChef
* Find/Replace (Find `\` replace with `space` (simple string))
* From Octal, space delimiter
* Remove Null Bytes

Take that output and save to a file. It is a Base64-encoded zlib/DEFLATE-compressed payload.

Run Python script against it and output to a file.
* GUI version in folder
```python
import base64, zlib

with open("Octal Decode.txt", "r") as f:
    data = f.read().strip()

decoded = zlib.decompress(base64.b64decode(data))
print(decoded.decode(errors="replace"))
```

Scroll down to Base64 encoded text.

Take back to Cyber Chef, and decode Base64.

Scroll down to find email address, which is reversed flag.

`}f7113307018770d52d4f94fec013197f{galf`

Into CyberChef, reverse function


## Flag
`flag{f791310cef49f4d25d0778107033117f}`


<p align="right">(<a href="#top">back to top</a>)</p>

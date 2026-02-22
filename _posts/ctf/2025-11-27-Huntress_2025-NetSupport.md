---
title: "NetSupport"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Malware"
  - "2025"
  - "PowerShell"
platform: Huntress 2025
competition_year: 2025
header:
  teaser: /assets/images/teasers/malware.png
toc: true
toc_sticky: true
---

# 🐞 NetSupport

|Category         |	Author                |
|-----------------|-----------------------|
|🐞 Malware       |Ben Folland            |

## Challenge Prompt

An unexpected Remote Monitoring and Management (RMM) tool was identified on this laptop. 
We identified a suspicious PowerShell script written to disk at a similar time. Can you find the link between the two?

## Problem Type
- PowerShell

## Password
The password to the ZIP archive is `netsupport`.
{: .notice--primary}


## Solve
Opened PowerShell file
Found byte string that PS converts to ZIP archive
```Powershell
[byte[]]$kKtyJL = $xϞzzghϞ + $mcZΛkϞπ + $RζπrMwGEQi
[IO.File]::WriteAllBytes($LjLSTACGF, $kKtyJL)
$weBRvm = [IO.Path]::ChangeExtension($LjLSTACGF, 'zip')
```
Used Python to save as ZIP:
```python
x = bytearray([80,75,3,4,20,0)]
with open('output_bytearray.zip', 'wb') as f:
    # Write the bytearray to the file
    f.write(x)
```

Expanded ZIP, and found INI file.<br>
Ran cat on `Client32.ini`

Found this line:
`Flag=ZmxhZ3tiNmU1NGQwYTBhNWYyMjkyNTg5YzM4NTJmMTkzMDg5MX0NCg==`

Base64 decode in CyberChef

## Flag
`flag{b6e54d0a0a5f2292589c3852f1930891}`


<p align="right">(<a href="#top">back to top</a>)</p>

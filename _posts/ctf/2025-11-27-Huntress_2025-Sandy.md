---
title: "Sandy"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Malware"
  - "2025"
  - "AutoIT"
platform: Huntress 2025
competition_year: 2025
header:
  teaser: /assets/images/teasers/malware.png
toc: true
toc_sticky: true
---

# 🐞 SANDY

|Category         |	Author                |
|-----------------|-----------------------|
|🐞 Malware       |John Hammond           |

## Challenge Prompt

My friend Sandy is really into cryptocurrencies! She's been trying to get me into it too, so she showed me a lot of Chrome extensions I could add to manage my wallets. Once I got everything sent up, she gave me this cool program!

She says it adds better protection so my wallets can't get messed with by hackers.

Sandy wouldn't lie to me, would she...? Sandy is the best!

This is the `Malware` category, and as such, includes malware.
Please be sure to analyze these files within an isolated virtual machine.
{: .notice--warning}


## Problem Type
- AutoIT

## Password
The password to the archive is `infected`.
{: .notice--primary}


## Solve
I started off by extracting the folder using the provided password. When I looked at the application inside, noted the icon which I wasn't familiar with:<br>
<img width="63" height="44" alt="2026-03-31_20-49-17" src="https://github.com/user-attachments/assets/01b8b621-815d-43f2-a6cd-acc8d2ca0c30" />

I threw that into Google Image search and that revealed that it was the logo for AutoIt:<br>
<img width="1396" height="909" alt="2026-03-31_20-50-38" src="https://github.com/user-attachments/assets/e0449831-e6eb-4fb6-8b35-33ec5aeaba2e" />

I next did a search for "AutoIt decomplie" which brought me to the [FAQ page for AutoIT](https://www.autoitscript.com/wiki/Decompiling_FAQ)<br>
> <H3>Is there a decompiler available?</H3>
> Yes, sort of. The official decompiler will only decompile scripts compiled with AutoIt v3.2.5.1 and earlier. Any script compiled with a version later than that will not decompile. 

Well lucky us, our script is 3.2.4.9.:<br>
<img width="405" height="566" alt="2026-03-31_20-49-01" src="https://github.com/user-attachments/assets/bef6fd65-963e-44ba-ade8-ab8bc6629c8d" />

Download and install [AutoIT 3.2.4.9](https://www.autoitscript.com/autoit3/files/archive/autoit/)

In the folder, inside the `Extras` folder, there is a tool called `Exe2Aut`. Run that and import the `SANDY.exe` file:<br>
<img width="1601" height="542" alt="2026-03-31_20-29-29" src="https://github.com/user-attachments/assets/de931079-342d-4369-94f3-95cdc9aa27c1" />

Run Exe2Aut to decomplie:<br>
<img width="596" height="359" alt="2026-03-31_20-29-36" src="https://github.com/user-attachments/assets/55700aaf-2ff8-4380-9935-1992a3fe5a0c" />

Next I opened the new `SANDY.au3` file in Visual Studio Code, but any text editor will work. I noticed on the sidebar there was a weird chunk unlike the rest and scrolled to that section:<br>
<img width="1854" height="897" alt="2026-03-31_20-32-49" src="https://github.com/user-attachments/assets/2f3e45d7-4bac-4926-8e65-cf0de7eb70a2" />


Next I copied all those Base64 pieces into CyberChef and used Find/replace for `"`, `&`, and `_` with nothing, remove whitespace, From Base64, and Remove Null Bytes:<br>
<img width="1534" height="884" alt="2026-03-31_20-34-44" src="https://github.com/user-attachments/assets/19d655af-4422-42af-8a7b-f96e18753374" />

Next I copied that back into CyberChef and removed all the find/replace recipies and left the From Base64 and Remove null bytes.

In the output of that I scrolled down and found the flag:<br>
<img width="1917" height="870" alt="2026-03-31_20-37-30" src="https://github.com/user-attachments/assets/333b8c10-025b-4467-be31-a26caa6dd3d9" />


## Flag
`flag{27768419fd176648b335aa92b8d2dab2}`


<p align="right">(<a href="#top">back to top</a>)</p>

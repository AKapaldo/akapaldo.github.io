---
title: "Deep Into My Heart"
date: 2026-02-17
categories:
  - THM
tags:
  - "Web"
  - "2026"
  - "Robots.txt"
platform: THM 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/web.png
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Deep Into my Heart

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt

My Dearest Hacker,

Cupid's Vault was designed to protect secrets meant to stay hidden forever. Unfortunately, Cupid underestimated how determined attackers can be.

Intelligence indicates that Cupid may have unintentionally left vulnerabilities in the system. With the holiday deadline approaching, you've been tasked with uncovering what's hidden inside the vault before it's too late.

You can find the web application here: `http://MACHINE_IP:5000`

## Problem Type
- Robots.txt

## Solve
I started off with a quick nmap scan of this host and port using `nmap -p 5000 -T4 -A IP_Address`.<br>
The very fist thing I noticed was a `robots.txt` file with a disallowed entry:
<img width="1355" height="435" alt="2026-02-14_17-03-15" src="https://github.com/user-attachments/assets/3fbca2ad-b45b-4ac8-be77-23c6ecb2eecf" />


If we look at the `robots.txt` file we see there is a `/cupids_secret_vault`:
<img width="683" height="169" alt="2026-02-14_17-04-40" src="https://github.com/user-attachments/assets/5e2e5350-2946-4fb2-b5b5-d80dbdc81dbe" />


When we visit that page we get the message that we discovered the secret vault but there is more to discover:
<img width="698" height="249" alt="2026-02-14_17-07-13" src="https://github.com/user-attachments/assets/1926e81f-fb25-4e07-bc2a-cb3ff5c8d696" />


The `robots.txt` file had a disallow for `/cupids_secret_vault/*` meaning there is probably another subdirectory.<br>
I like `ffuf` so I used `ffuf -w /usr/share/wordlist/dirbuster/directory-list-2.3-small.txt:FUZZ -u http://<SITE IP HERE>:5000/cupids_secret_vault/FUZZ`.<br>
This found an administrator subdirectory:
<img width="1100" height="616" alt="2026-02-14_17-14-07" src="https://github.com/user-attachments/assets/32f818ff-a0ab-4594-b570-e8aef772ce1d" />


On the `http://SITE_IP:5000/cupids_secret_vault/administrator` page we get a login in prompt:
<img width="579" height="435" alt="2026-02-14_17-14-52" src="https://github.com/user-attachments/assets/da55161f-0f80-4566-bcdd-ed80747a2f73" />


At first I tried the obvious `administrator` for the username and back on the `robots.txt` file we had a comment of `cupid_arrow_2026!!!` which I used for the password.<br>
This surprisingly didn't work, but changing the username to `admin` and using the password from the `robots.txt` file did the trick:
<img width="930" height="401" alt="2026-02-14_17-19-14" src="https://github.com/user-attachments/assets/17d13b6e-50f9-4340-8e73-57308b4f2ba9" />


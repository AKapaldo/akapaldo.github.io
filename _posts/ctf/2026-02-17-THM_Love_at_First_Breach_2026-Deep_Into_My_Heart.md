---
title: "Deep Into My Heart"
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

# 🌐 Love at First Breach 2026 - Deep Into my Heart

|Category         |	Author                |
|-----------------|-----------------------|
| 🌐 Web         |TryHackMe      |

## Challenge Prompt

My Dearest Hacker,

Cupid's Vault was designed to protect secrets meant to stay hidden forever. Unfortunately, Cupid underestimated how determined attackers can be.

Intelligence indicates that Cupid may have unintentionally left vulnerabilities in the system. With the holiday deadline approaching, you've been tasked with uncovering what's hidden inside the vault before it's too late.

You can find the web application here: `http://MACHINE_IP:5000`

## Problem Type
- Web

## Solve
I started off with a quick nmap scan of this host and port using `nmap -p 5000 -T4 -A <IP Address>`.<br>
The very fist thing I noticed was a `robots.txt` file with a disallowed entry:
<img width="1355" height="435" alt="image" src="https://github.com/user-attachments/assets/fdc96f68-e801-4b0a-9cc5-fb1cbd45bdaa" />

If we look at the `robots.txt` file we see there is a `/cupids_secret_vault`:
<img width="683" height="169" alt="image" src="https://github.com/user-attachments/assets/b6cb0830-b752-4df9-8bbc-3d9ae05e4df3" />

When we visit that page we get the message that we discovered the secret vault but there is more to discover:
<img width="698" height="249" alt="image" src="https://github.com/user-attachments/assets/377f71d0-33a4-47d0-8b30-3d128735a24d" />

The `robots.txt` file had a disallow for `/cupids_secret_vault/*` meaning there is probably another subdirectory.<br>
I like `ffuf` so I used `ffuf -w /usr/share/wordlist/dirbuster/directory-list-2.3-small.txt:FUZZ -u http://<SITE IP HERE>:5000/cupids_secret_vault/FUZZ`.<br>
This found an administrator subdirectory:
<img width="1100" height="616" alt="image" src="https://github.com/user-attachments/assets/11be139a-9e9a-4b8c-85c7-b6380dbc6ace" />

On the `http://<SITE IP HERE>:5000/cupids_secret_vault/administrator` page we get a login in prompt:
<img width="579" height="435" alt="image" src="https://github.com/user-attachments/assets/ab940594-cf42-458f-a6d6-9f2386d96111" />

At first I tried the obvious `administrator` for the username and back on the `robots.txt` file we had a comment of `cupid_arrow_2026!!!` which I used for the password.<br>
This surprisingly didn't work, but changing the username to `admin` and using the password from the `robots.txt` file did the trick:
<img width="930" height="401" alt="image" src="https://github.com/user-attachments/assets/417788b3-3867-487a-b258-551257f7bbb0" />

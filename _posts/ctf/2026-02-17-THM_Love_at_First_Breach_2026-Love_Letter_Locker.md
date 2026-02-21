---
title: "Love Letter Locker"
date: 2026-02-17
categories:
  - THM_Love_at_First_Breach_2026
tags:
  - "Web"
        - "2026"
        - "IDOR"
platform: THM Love at First Breach 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Love Letter Locker

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt

My Dearest Hacker,

Welcome to LoverLetterLocker, where you can safely write and store your Valentine's letters. For your eyes only?

You can access the web app here: `http://MACHINE_IP:5000`

## Problem Type
- IDOR

## Solve
Upon visitng the page, we have 2 options to log in or create an account.<br>
So, let's make a new account.
<img width="1917" height="350" alt="2026-02-15_13-35-10" src="https://github.com/user-attachments/assets/88e0bb9c-07f8-4eb2-a7b9-ce911227536f" />


I made my account and was presented with the dashboard. The "Tip from Cupid" that states every love letter gets a unique number made me instantly think this might be an Insecure Direct Object Reference (IDOR) problem.<br>
We can also see there are already 2 other letters in the archive.<br>
<img width="1914" height="502" alt="2026-02-15_13-36-11" src="https://github.com/user-attachments/assets/3fa6cfb3-939c-4b6e-8f5d-eb71d1635f2f" />


Next I made a new letter with the title and message of `Test`.
<img width="1914" height="569" alt="2026-02-15_13-36-26" src="https://github.com/user-attachments/assets/ca0a042a-ecfc-4732-95d8-48f27af2db0c" />


When we save our letter, we are taken to the view page where we can see we are letter #3 and that is refected in the URL. If we change the 3 to 2, can we see another letter?
<img width="1911" height="467" alt="2026-02-15_13-36-38" src="https://github.com/user-attachments/assets/c88e357e-ca8b-4205-b713-e1a520857bdc" />


When we change the 3 to a 2 in the URL and submit we do in fact see a different letter:
<img width="1917" height="432" alt="2026-02-15_13-36-48" src="https://github.com/user-attachments/assets/932f22af-a058-44fc-a6a4-37716f466043" />


When we look at letter #1, we see the flag:
<img width="1917" height="468" alt="2026-02-15_13-36-58" src="https://github.com/user-attachments/assets/8533dd00-2b67-4e2a-b925-771699edd8c9" />

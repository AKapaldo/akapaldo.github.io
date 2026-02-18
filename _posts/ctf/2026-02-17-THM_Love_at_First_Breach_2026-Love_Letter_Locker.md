---
title: "Love Letter Locker"
date: 2026-02-17
categories:
  - THM
tags:
  - Web
  - IDOR
  - "2026"
platform: THM Love at First Breach 2026
competition_year: "2026"
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
- Web
- IDOR

## Solve
Upon visitng the page, we have 2 options to log in or create an account.<br>
So, let's make a new account.
<img width="1917" height="350" alt="image" src="https://github.com/user-attachments/assets/7b640c04-5f6f-4310-8ff7-b77026b1652c" />

I made my account and was presented with the dashboard. The "Tip from Cupid" that states every love letter gets a unique message made me instantly think this might be an Insecure Direct Object Reference (IDOR) problem.<br>
We can also see there are already 2 other letters in the archive.<br>
<img width="1914" height="502" alt="image" src="https://github.com/user-attachments/assets/7a3d0286-7c87-4037-988d-1108ced428e4" />

Next I made a new letter with the title and message of `Test`.
<img width="1914" height="569" alt="image" src="https://github.com/user-attachments/assets/aeaede5a-3d89-4b21-8435-3ea1c9792055" />

When we save our letter, we are taken to the view page where we can see we are letter #3 and that is refected in the URL. If we change the 3 to 2, can we see another letter?
<img width="1911" height="467" alt="image" src="https://github.com/user-attachments/assets/34a4ea72-844c-428f-adf7-8b84f1f4789d" />

When we change the 3 to a 2 in the URL and submit we do in fact see a different letter:
<img width="1917" height="432" alt="image" src="https://github.com/user-attachments/assets/152839b8-8898-4178-99f6-f709812d39b1" />

When we look at letter #1, we see the flag:
<img width="1917" height="468" alt="image" src="https://github.com/user-attachments/assets/46ed7700-1447-4c2d-b2ac-cfa6a5758392" />

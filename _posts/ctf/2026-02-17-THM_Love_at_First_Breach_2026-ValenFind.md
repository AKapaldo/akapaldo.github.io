---
title: "ValenFind"
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

# 🌐 Love at First Breach 2026 - Valenfind

|Category         |	Author                |
|-----------------|-----------------------|
| 🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

There’s this new dating app called “Valenfind” that just popped up out of nowhere. I hear the creator only learned to code this year; surely this must be vibe-coded. Can you exploit it?

You can access it here: `http://MACHINE_IP:5000`

## Problem Type
- Web
- LFI


## Solve
We will start by running a quick nmap scan on the host and port to see what is running on the IP and port with<br> `nmap -p 5000 -T4 -A <IP ADDRESS>`:

<img width="1349" height="396" alt="image" src="https://github.com/user-attachments/assets/fc3b86f7-f4dc-4146-83b2-bb6331f8ca62" />

This shows we have Python running, so maybe a Flask app.

Next we will visit the site as instructed. When we load it up, we see the ValenFind app:
<img width="1916" height="375" alt="image" src="https://github.com/user-attachments/assets/392dc352-94e3-423f-909b-bff7e7a46369" />

If we click the `Start Your Journey` button we can create an account: 
<img width="1916" height="455" alt="image" src="https://github.com/user-attachments/assets/45ac7a2f-9550-4ffd-8a44-f847c060cefc" />

I used `a` for my username and `password123`, just so it was easy to remember. Then we are taken to complete our profile. We can fill that out and then click `Finish Profile & Start Dating`:
<img width="882" height="772" alt="image" src="https://github.com/user-attachments/assets/3b84b802-64c4-4fff-8977-8c7265fd695e" />

On the next page, we are given Potential Matches. Scrolling through the list the `Cupid` user looks most interesting with 999 likes:
<img width="894" height="772" alt="image" src="https://github.com/user-attachments/assets/262ce627-dba9-40d5-8662-500c772e2a09" />

If we click the `Profile` button, we can see this user "Keeps the Database Secure".
<img width="678" height="578" alt="image" src="https://github.com/user-attachments/assets/09846e8f-ccb7-4622-b85c-9f0527442574" />

Let's see if there is anything interesting here. If we right click and pick "View Page Source", part way down in the JavaScript, there is a comment labeled `Vulnerability`.
<img width="713" height="420" alt="image" src="https://github.com/user-attachments/assets/7e3e59e7-d0d7-4414-afa1-bae4bf10e220" />

It seems the `layout` parameter allows for LFI (Local File Inclusion)! Let's fire up Burpsuite and see if we can exploit that.

I use FoxyProxy in Firefox, but you can use the built in browser in Burp if you like. We will send a request and intercept it, then send that to repeater so we can modify it.
<img width="1831" height="904" alt="image" src="https://github.com/user-attachments/assets/0082790e-f128-47d0-a0df-8e61ef44f406" />

Since I captured a `POST` request, I had to change mine over to a `GET` instead and the added the URL we want to try to abuse: `/api/fetch_layout?layout=`. I tested with the `/etc/passwd` file and as you can see this works and we are provided the file.
<img width="1588" height="898" alt="image" src="https://github.com/user-attachments/assets/01e6d162-dd00-4ac9-b745-2d72eb8d037d" />

The next thing I tried was to send something that shouldn't work in this case, just 2 periods (`..`). This returns an error that it couldn't load `/opt/Valenfind/templates/components/..` with the `..` being what we sent:
<img width="1601" height="901" alt="image" src="https://github.com/user-attachments/assets/d145afa7-3b3e-45a3-aab1-b5369a349edc" />

Then we can take the file path we found and try to add `app.py` to the end since as we found at the start, this may be a Flask app. This results in a error that the file wasn't found:
<img width="1583" height="897" alt="image" src="https://github.com/user-attachments/assets/4c52c00e-d7bd-47bd-a65f-b11d9ea7e9a6" />

I worked my way down the file path until I got to `/opt/Valenfind/app.py` and there we got a return! On line 17 we see an Admin API key too!
<img width="1601" height="898" alt="image" src="https://github.com/user-attachments/assets/e0a18bea-19bc-41e8-98eb-ae83cb71a519" />

As I read through the rest of the code, on line 229 I found an `export_db` API function. This function requires a special header (Line 231) `X-Valentine-Token` that must match the `ADMIN_API_KEY` (line 233) from above.
<img width="1593" height="897" alt="image" src="https://github.com/user-attachments/assets/106b674d-20b0-425a-abe2-45efa1b32b19" />

Trying that gives us the flag!
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/0ec1e1d9-fc36-44c2-946f-c3400e33ef50" />

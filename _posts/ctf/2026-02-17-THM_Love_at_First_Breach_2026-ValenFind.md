---
title: "ValenFind"
date: 2026-02-17
categories:
  - THM_Love_at_First_Breach
tags:
  - "Web"
  - "2026"
  - "Local File Inclusion (LFI)"
platform: THM Love at First Breach 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - Valenfind

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

There’s this new dating app called “Valenfind” that just popped up out of nowhere. I hear the creator only learned to code this year; surely this must be vibe-coded. Can you exploit it?

You can access it here: `http://MACHINE_IP:5000`

## Problem Type
- Local File Inclusion (LFI)


## Solve
We will start by running a quick nmap scan on the host and port to see what is running on the IP and port with<br> `nmap -p 5000 -T4 -A IP_ADDRESS`:
<img width="1349" height="396" alt="2026-02-14_16-35-45" src="https://github.com/user-attachments/assets/c85b02fd-7eb7-4ea7-8ca8-d1fdb7830ad1" />


This shows we have Python running, so maybe a Flask app.

Next we will visit the site as instructed. When we load it up, we see the ValenFind app:
<img width="1916" height="375" alt="2026-02-14_16-01-53" src="https://github.com/user-attachments/assets/876f89db-d3fc-4ce4-bc30-28c650f87954" />


If we click the `Start Your Journey` button we can create an account: 
<img width="1916" height="455" alt="2026-02-14_16-02-50" src="https://github.com/user-attachments/assets/da1eb481-50b2-40fd-be5b-10ae33fb6d04" />


I used `a` for my username and `password123`, just so it was easy to remember. Then we are taken to complete our profile. We can fill that out and then click `Finish Profile & Start Dating`:
<img width="882" height="772" alt="2026-02-14_16-04-19" src="https://github.com/user-attachments/assets/4730ea09-59dc-4494-854a-ed5baead6835" />


On the next page, we are given Potential Matches. Scrolling through the list the `Cupid` user looks most interesting with 999 likes:
<img width="894" height="772" alt="2026-02-14_16-06-13" src="https://github.com/user-attachments/assets/5c7e7157-d462-4267-a962-463f9bf0b148" />


If we click the `Profile` button, we can see this user "Keeps the Database Secure".
<img width="678" height="578" alt="2026-02-14_16-07-12" src="https://github.com/user-attachments/assets/cf0b4c3d-99a6-47b7-a05e-18f0a164d3cf" />


Let's see if there is anything interesting here. If we right click and pick "View Page Source", part way down in the JavaScript, there is a comment labeled `Vulnerability`.

<img width="713" height="420" alt="2026-02-14_16-08-49" src="https://github.com/user-attachments/assets/95711228-3d29-40fc-a36b-720c6991fc39" />

It seems the `layout` parameter allows for LFI (Local File Inclusion)! Let's fire up Burpsuite and see if we can exploit that.

I use FoxyProxy in Firefox, but you can use the built in browser in Burp if you like. We will send a request and intercept it, then send that to repeater so we can modify it.
<img width="1831" height="904" alt="2026-02-14_16-22-40" src="https://github.com/user-attachments/assets/a73290c0-c51e-47a5-94f6-41b47d25a59f" />


Since I captured a `POST` request, I had to change mine over to a `GET` instead and the added the URL we want to try to abuse: `/api/fetch_layout?layout=`. I tested with the `/etc/passwd` file and as you can see this works and we are provided the file.
<img width="1588" height="898" alt="2026-02-14_16-28-37" src="https://github.com/user-attachments/assets/64f78cc7-4fb1-4ba7-9ace-44521de564ff" />


The next thing I tried was to send something that shouldn't work in this case, just 2 periods (`..`). This returns an error that it couldn't load `/opt/Valenfind/templates/components/..` with the `..` being what we sent:
<img width="1601" height="901" alt="2026-02-14_16-32-40" src="https://github.com/user-attachments/assets/2f42d6c9-8646-4cac-b618-893e23ac8321" />


Then we can take the file path we found and try to add `app.py` to the end since as we found at the start, this may be a Flask app. This results in a error that the file wasn't found:
<img width="1583" height="897" alt="2026-02-14_16-38-03" src="https://github.com/user-attachments/assets/462403fc-c02e-496e-950e-6823de46f575" />


I worked my way down the file path until I got to `/opt/Valenfind/app.py` and there we got a return! On line 17 we see an Admin API key too!
<img width="1601" height="898" alt="2026-02-14_16-42-18" src="https://github.com/user-attachments/assets/82a3eb54-b792-41e7-a3ed-d5e28be59800" />


As I read through the rest of the code, on line 229 I found an `export_db` API function. This function requires a special header (Line 231) `X-Valentine-Token` that must match the `ADMIN_API_KEY` (line 233) from above.
<img width="1593" height="897" alt="2026-02-14_16-43-24" src="https://github.com/user-attachments/assets/65916a08-4d4a-4ef0-ba72-3e7a3584a18a" />


Trying that gives us the flag!
<img width="1599" height="899" alt="2026-02-14_16-48-57" src="https://github.com/user-attachments/assets/c819e361-8e0e-4e20-bd03-e4ebe8158ee3" />


---
title: "Try Heart Me"
date: 2026-02-17
categories:
  - THM_Love_at_First_Breach_2026
tags:
  - "Web"
        - "2026"
        - "JSON Web Token (JWT)"
platform: THM Love at First Breach 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - TryHeartMe

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

The TryHeartMe shop is open for business. Can you find a way to purchase the hidden “Valenflag” item? <br>
You can access the web app here: `http://MACHINE_IP:5000`


## Problem Type
- JSON Web Token (JWT)

## Solve
When we visit the website, we are presented with the TryHeartMe Valentines Shop. Four items are displayed, but none of them the `ValenFlag` item we want.<br>
<img width="1899" height="804" alt="2026-02-14_20-46-17" src="https://github.com/user-attachments/assets/fbf06bf7-911a-4f61-ae7c-d3a264fe0b52" />



When we try to make a purchase it tells us we must sign up first, so let's start with that.
Click the `Sign Up` button and put in an email and password. I used `a@a.com` and `password`.

We start off with 0 credits, so let's jump into Burpsuite to see if we can modify our credits.<br>
If we turn on FoxyProxy in Firefox and intercept a reload of the page, we can see our cookie is a tryheartme_jwt. JWT is a JSON Web Token.
<img width="1589" height="892" alt="2026-02-14_20-53-31" src="https://github.com/user-attachments/assets/e9129235-1889-4c51-b4d5-1279a27f396f" />


Our Base64 encoded token is:
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImFAYS5jb20iLCJyb2xlIjoidXNlciIsImNyZWRpdHMiOjAsImlhdCI6MTc3MTEyMTI3NCwidGhlbWUiOiJ2YWxlbnRpbmUifQ.3Vo7afsoEFai85jrVsUyURwkL95NNVtzal2ROXhu140`

If we take that into [jwt.io](https://jwt.io) we can use the decoder to see what is hidden in the token. We see that our role is `user` and our credits are `0`:
<img width="1563" height="629" alt="2026-02-14_21-10-38" src="https://github.com/user-attachments/assets/a314d59d-4096-400b-beff-b66ba9b754b7" />


Let's use the encoder function to make us an `admin` and set our credits to `9999`:
<img width="1375" height="632" alt="2026-02-14_21-15-05" src="https://github.com/user-attachments/assets/241194bd-162e-4471-8cea-296ea3218106" />


Then we can use the copy button to copy our token and go back to our page. If we press `F12` on the keyboard to jump into Developer mode, we can then go to `Storage` (1), Expand `Cookies` (2) , and then paste our new JWT into the `Value` box (3).
<img width="1907" height="807" alt="2026-02-14_21-15-54" src="https://github.com/user-attachments/assets/da72f593-d5c9-4e61-b421-cedeac3f7bb3" />


Then we reload the page and now we have the `ValenFlag` available for 777 credits and lucky us, we have 9999.
<img width="1916" height="811" alt="2026-02-14_21-19-09" src="https://github.com/user-attachments/assets/e45cb0f3-4534-485f-a4c6-d363150c6bac" />


If we click on the ValenFlag and click `Buy`, we are presented with the flag on the next screen!
<img width="1917" height="505" alt="2026-02-14_21-19-56" src="https://github.com/user-attachments/assets/4a27a0de-12f9-4abc-9bdb-2ebf3ce0f110" />


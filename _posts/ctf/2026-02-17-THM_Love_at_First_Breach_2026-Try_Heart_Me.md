---
title: "Try Heart Me"
date: 2026-02-17
categories:
  - THM
tags:
  - Web
  - JWT
  - "2026"
platform: THM Love at First Breach 2026
competition_year: "2026"
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
- Web
- JWT

## Solve
When we visit the website, we are presented with the TryHeartMe Valentines Shop. Four items are displayed, but none of them the `ValenFlag` item we want.<br>
<img width="1899" height="804" alt="image" src="https://github.com/user-attachments/assets/a3b55525-9829-460b-85d2-9d1af9bb5459" />


When we try to make a purchase it tells us we must sign up first, so let's start with that.
Click the `Sign Up` button and put in an email and password. I used `a@a.com` and `password`.

We start off with 0 credits, so let's jump into Burpsuite to see if we can modify our credits.<br>
If we turn on FoxyProxy in Firefox and intercept a reload of the page, we can see our cookie is a tryheartme_jwt. JWT is a JSON Web Token and this appears to be Base64 encoded.
<img width="1589" height="892" alt="image" src="https://github.com/user-attachments/assets/0bf4f7cd-1359-4950-b798-a2f3a00a7362" />

Our Base64 encoded token is:
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImFAYS5jb20iLCJyb2xlIjoidXNlciIsImNyZWRpdHMiOjAsImlhdCI6MTc3MTEyMTI3NCwidGhlbWUiOiJ2YWxlbnRpbmUifQ.3Vo7afsoEFai85jrVsUyURwkL95NNVtzal2ROXhu140`

If we take that into [jwt.io](https://jwt.io) we can use the decoder to see what is hidden in the token. We see that our role is `user` and our credits are `0`:
<img width="1563" height="629" alt="image" src="https://github.com/user-attachments/assets/9d0a389f-1818-4aa4-a53c-03487addced3" />

Let's use the encoder function to make us an `admin` and set our credits to `9999`:
<img width="1375" height="632" alt="image" src="https://github.com/user-attachments/assets/74e198bf-2c1d-4033-88c8-2b22db6e318a" />

Then we can use the copy button to copy our token and go back to our page. If we press `F12` on the keyboard to jump into Developer mode, we can then go to `Storage` (1), Expand `Cookies` (2) , and then paste our new JWT into the `Value` box (3).
<img width="1907" height="807" alt="image" src="https://github.com/user-attachments/assets/72183797-1101-4493-a106-697052155615" />

Then we reload the page and now we have the `ValenFlag` available for 777 credits and lucky us, we have 9999.
<img width="1916" height="811" alt="image" src="https://github.com/user-attachments/assets/4b5ac8fc-866e-4aaf-8eec-d7fe4049e73a" />

If we click on the ValenFlag and click `Buy`, we are presented with the flag on the next screen!
<img width="1917" height="505" alt="image" src="https://github.com/user-attachments/assets/9dc53879-fc02-4c19-ae89-088734210e73" />

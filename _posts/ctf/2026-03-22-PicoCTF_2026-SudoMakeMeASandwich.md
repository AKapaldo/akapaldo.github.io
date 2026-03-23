---
title: "SudoMakeMeASandwich"
date: 2026-03-22
categories:
  - PicoCTF
tags:
  - "Miscellaneous"
  - "2026"
  - "Sudo"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 📦 SUDO MAKE ME A SANDWICH

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous         |Darkraicg492        |

## Challenge Prompt

Can you read the flag? I think you can!

## Problem Type
- Sudo


## Solve
We are told we need to SSH into the machine, so we start with that.
<img width="850" height="521" alt="2026-03-22_13-32-56" src="https://github.com/user-attachments/assets/cb64ca05-830e-483e-87ef-b77945ed6ead" />

Upon logging in we check the directory where we see that there is a `flag.txt` file, but it is read only by Root.
<img width="555" height="174" alt="2026-03-22_13-33-13" src="https://github.com/user-attachments/assets/bddf1754-c37c-419c-91dd-1e51c0cef4b1" />

If we run a `sudo -l` to list out the commands we can run as root we see we can run `emacs` with no password.<br>
<img width="969" height="117" alt="2026-03-22_13-33-38" src="https://github.com/user-attachments/assets/856a1853-9960-4f60-8b87-9a0bac1a5d12" />

So, let's use that to read the file!<br>
<img width="380" height="46" alt="2026-03-22_13-35-16" src="https://github.com/user-attachments/assets/69980443-237e-4a67-93fd-1361929e4c0a" />

Inside the `flag.txt` file we have the flag!<br>
<img width="429" height="152" alt="2026-03-22_13-34-31" src="https://github.com/user-attachments/assets/fd2bf145-c216-43c3-923a-5110a8811d0b" />

To exit emacs, you can use `Control + X` and then `Control + C`

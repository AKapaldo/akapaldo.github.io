---
title: "bytemancy2"
date: 2026-03-21
categories:
  - PicoCTF
tags:
  - "Binary"
  - "2026"
  - "Python"
  - "pwntools"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/binary.png
toc: true
toc_sticky: true
---

# ⚒️ Bytemancy 2

|Category         |	Author                |
|-----------------|-----------------------|
|⚒️ Binary Exploitation       |LT 'syreal' Jones    |

## Challenge Prompt

Can you conjure the right bytes? The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_lonely_island/19e324d00cff4e033f56b6f0abc222edd18c5bb988d55147d04e21e5ace1be14/app.py).

Additional details will be available after launching your challenge instance.

## Problem Type
- Python
- pwntools

## Solve
We are given the program source code:
```python
import sys

while(True):
  try:
    print('⊹──────[ BYTEMANCY-2 ]──────⊹')
    print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
    print()
    print('Send me the HEX BYTE 0xFF 3 times, side-by-side, no space.')
    print()
    print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
    print('⊹─────────────⟡─────────────⊹')
    print('==> ', end='', flush=True)
    user_input = sys.stdin.buffer.readline().rstrip(b"\n")
    if user_input == b"\xff\xff\xff":
      print(open("./flag.txt", "r").read())
      break
    else:
      print("That wasn't it. I got: " + str(user_input))
      print()
      print()
      print()
  except Exception as e:
    print(e)
    break

```

This time we need to send the program `\xff\xff\xff`.
What may be a little misleading is [CyberChef](https://gchq.github.io/CyberChef/) makes that look like a printable character, but if you look at the hints you will note it is not.
<img width="1051" height="280" alt="2026-03-21_13-35-11" src="https://github.com/user-attachments/assets/4bc6c39c-f822-43de-b415-6bb30d5d687a" />

To solve this, we will need to use pwntools.<br>
If you don't have pwntools you will need to install it, it doens't come with Kali by default.
You can install using:
```bash
sudo apt update
pipx install pwntools
pipx ensurepath
```

For our script we will use 
```python
from pwn import *

HOST = 'lonely-island.picoctf.net'
PORT = 64948

p = remote(HOST, PORT)

p.recvuntil(b'==> ')
p.sendline(b"\xff\xff\xff")

print(p.recvall().decode())
```

Make sure you use your host and port from the challenge instance you are given:
<img width="1174" height="485" alt="2026-03-21_13-32-21" src="https://github.com/user-attachments/assets/8f0e9108-82f7-4d6f-96fe-2842a5502642" />

We can run the program and when we do that, we are presented with the flag!
<img width="779" height="263" alt="2026-03-21_13-37-28" src="https://github.com/user-attachments/assets/9ab93ab6-3af5-4ab8-85e9-f8f5da1dc88c" />


<p align="right">(<a href="#top">back to top</a>)</p>


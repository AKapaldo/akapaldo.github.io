---
title: "bytemancy3"
date: 2026-03-22
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

# ⚒️ Bytemancy 3

|Category         |	Author                |
|-----------------|-----------------------|
|⚒️ Binary Exploitation       |LT 'syreal' Jones    |

## Challenge Prompt

Can you conjure the right bytes? The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_green_hill/8a399c627aef260e42d722530b1051c9f32d8c5731508e7dcfb87b6cc2ea68f5/app.py) and the compiled spellbook binary can be downloaded [here](https://challenge-files.picoctf.net/c_green_hill/8a399c627aef260e42d722530b1051c9f32d8c5731508e7dcfb87b6cc2ea68f5/spellbook).

Additional details will be available after launching your challenge instance.

## Problem Type
- Python
- pwntools

## Solve
We are given the program source code:
```python
import os
import random
import select
import sys
from typing import Optional
from pwn import ELF, p32

BANNER = "⊹──────[ BYTEMANCY-3 ]──────⊹"
BINARY_PATH = os.path.join(os.path.dirname(__file__), "spellbook")
QUESTION_COUNT = 3

SPELLBOOK_FUNCTIONS = [
    "ember_sigil",
    "glyph_conflux",
    "astral_spark",
    "binding_word",
]


def read_exact_bytes(expected_len: int) -> Optional[bytes]:
    """Read a fixed number of bytes from stdin, trimming a trailing newline."""
    stdin_buffer = sys.stdin.buffer
    buf = stdin_buffer.read(expected_len)
    if not buf or len(buf) < expected_len:
        return None

    # Discard trailing newlines only if more bytes are immediately available
    try:
        stdin_fileno = sys.stdin.fileno()
    except (AttributeError, ValueError, OSError):
        stdin_fileno = None

    if stdin_fileno is not None:
        while True:
            readable, _, _ = select.select([stdin_fileno], [], [], 0)
            if not readable:
                break

            peek = stdin_buffer.peek(1)[:1]
            if peek in (b"\n", b"\r"):
                stdin_buffer.read(1)
                continue
            break

    return buf


def main():
    try:
        elf = ELF(BINARY_PATH, checksec=False)
    except FileNotFoundError:
        print("The spellbook is missing!")
        return

    flag = open("./flag.txt", "r").read().strip()

    while True:
        try:
            print(BANNER)
            print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
            print()
            print("I will name four procedures hidden inside spellbook.")
            print(
                f"Each round, send me their *raw* 4-byte addresses "
                f"in little-endian form. {QUESTION_COUNT} correct answers unlock the flag."
            )
            print()
            print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
            print('⊹─────────────⟡─────────────⊹')

            selections = random.sample(SPELLBOOK_FUNCTIONS, QUESTION_COUNT)
            success = True

            for idx, symbol in enumerate(selections, 1):
                target_addr = elf.symbols[symbol]
                expected_bytes = p32(target_addr)

                print(
                    f"[{idx}/{QUESTION_COUNT}] Send the 4-byte little-endian "
                    f"address for procedure '{symbol}'."
                )
                print("==> ", end='', flush=True)
                user_bytes = read_exact_bytes(len(expected_bytes))

                if user_bytes is None:
                    print("\nI needed four bytes, traveler.")
                    success = False
                    break

                if user_bytes != expected_bytes:
                    print("\nThose aren't the right runes.")
                    success = False
                    break

            if success:
                print(flag)
                break

            print()
            print("The aether rejects your incantation. Try again.\n")
        except EOFError:
            break
        except Exception as exc:
            print(exc)
            break


if __name__ == "__main__":
    main()
```

This time we need to send the program the exact raw 4 byte address in little-endian of 3 random spells from the spellbook app.<br>
To get the locations we need to feed the app, we can use:
```bash
objdump -t spellbook
```
Near the bottom of the output we can see the 4 functions we need addresses for:
<img width="793" height="768" alt="image" src="https://github.com/user-attachments/assets/778e0eb3-6e60-40e9-9093-c883475bd52e" />


To solve this, we will need to use pwntools.<br>
If you don't have pwntools you will need to install it, it doesn't come with Kali by default.
You can install using:
```bash
sudo apt update
pipx install pwntools
pipx ensurepath
```

Connecting to the machine we can see it gives us a random spell from the spellbook, so we can write a pwntools script to look for that and then send the right location:
<img width="905" height="229" alt="image" src="https://github.com/user-attachments/assets/0eb5238f-b4e1-4e71-85b2-1bf3349f92e9" />


For our script we will use the below. This will read in our prompt and look for the matching spellbook item before sending the location:
```python
from pwn import *

HOST = 'green-hill.picoctf.net'
PORT = 49782

p = remote(HOST, PORT)

responses = {
    b"ember_sigil": p32(0x08049176),
    b"glyph_conflux": p32(0x0804919a),
    b"binding_word": p32(0x080491e3),
    b"astral_spark": p32(0x080491c1)
}

for _ in range(3):
    data = p.recvuntil(b'==> ')
    
    for prompt, payload in responses.items():
        if prompt in data:
            p.send(payload)
            break

print(p.recvall().decode(errors="ignore"))
```

`p32` packs a 32-bit integer and since we didn't specify, it uses little endian. If we needed big endian, we could specify that like this:<br>
<img width="580" height="163" alt="image" src="https://github.com/user-attachments/assets/74550f5e-51a3-4cb9-a843-86573e1dc8f5" />

Make sure you use your host and port from the challenge instance you are given:
<img width="954" height="443" alt="image" src="https://github.com/user-attachments/assets/8ed0ba8c-6f5f-4579-b365-d41e3e09d0e9" />

We can run the program and when we do that, we are presented with the flag!
<img width="594" height="135" alt="image" src="https://github.com/user-attachments/assets/8a43b920-cb46-4900-bc9f-ba16147302af" />

---
title: "Bytemancy1"
date: 2026-03-21
categories:
  - PicoCTF
tags:
  - "Binary"
  - "2026"
  - "Python"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/binary.png
toc: true
toc_sticky: true
---

# ⚒️ Bytemancy 1

|Category         |	Author                |
|-----------------|-----------------------|
|⚒️ Binary Exploitation       |LT 'syreal' Jones    |

## Challenge Prompt

Can you conjure the right bytes? The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_foggy_cliff/b04a1829da5f649435a6903647d126ba60a3d04a250e8f7e3973b2ce121beba5/app.py).

Additional details will be available after launching your challenge instance.

## Problem Type
- Python

## Solve
We are given the program source code:
```python
while(True):
  try:
    print('⊹──────[ BYTEMANCY-1 ]──────⊹')
    print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
    print()
    print('Send me ASCII DECIMAL 101 1751 times, side-by-side, no space.')
    print()
    print("☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐")
    print('⊹─────────────⟡─────────────⊹')
    user_input = input('==> ')
    if user_input == "\x65"*1751:
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

This time we need to send the program 1,751 \x65 characters.
Again remembering from Bytemancy0 we saw in [CyberChef](https://gchq.github.io/CyberChef/) that \x65 is just `e`. <br>
<img width="797" height="216" alt="2026-03-10_19-57-54" src="https://github.com/user-attachments/assets/73ffdffb-0c20-4bea-a63b-97537ab045e0" />

We could type out the letter `e` 1,751 times and send that, but we will make it a little eaiser on ourselves and use python.

For our script we will use:
```bash
python3 -c "print('e' * 1751)" | xclip -selection clipboard
```
This will generate 1,751 `e` characters and copy it directly to our clipboard to paste.

We can then `nc` to the machine and paste our `e` characters into the box:
<img width="1313" height="453" alt="2026-03-21_13-22-40" src="https://github.com/user-attachments/assets/0a26c6a8-dffa-40ff-a29f-9e0403a60104" />

When we do that, we are presented with the flag!

<p align="right">(<a href="#top">back to top</a>)</p>


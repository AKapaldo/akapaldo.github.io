---
title: "VX-Underground"
date: 2025-11-26
categories:
  - Huntress
tags:
  - Miscellaneous
  - "2025"
platform: Huntress 2025
competition_year: "2025"
toc: true
toc_sticky: true
---

# 📦 vx-underground

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |John Hammond           |

## Challenge Prompt

vx-underground, widely known across social media for hosting the largest collection and library of cat pictures, has been plagued since the dawn of time by people asking: "what's the password?"

Today, we ask the same question. We believe there are secrets shared amongst the cat pictures... but perhaps these also lead to just more cats.

Uncover the flag from the file provided.

## Problem Type
- Shamir's Secret Sharing

## Solve
Folder contained many cat pictures and top level had a single cat picture called Prime_Mod.jpg.

Ran:
```bash
exiftool prime_mod.jpg
```

Found a "User Comment":<br>
Prime Modulus: 010000000000000000000000000000000000000000000000000000000000000129

Each picture had a User Comment like:
`111-217a2437dd04b895a758d562b089263808d41ca14a1d5634e142a3c3dbd716ff`

I went to the folder of cat pictures and ran, to pull the user comments, remove the begining, order them, and save to a file:
```bash
exiftool -a * | grep "User Comment" | awk '{print $4}' | sort -t- -k1,1n | sed 's/-/ /g' | awk '{print $2}' > data.txt
```

Found out about [Shamir's Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_secret_sharing).

> [!NOTE]
> SSS is used to secure a secret in a distributed form, most often to secure encryption keys.
> The secret is split into multiple shares, which individually do not give any information about the secret.
> To reconstruct a secret secured by SSS, a number of shares is needed, called the threshold.

Use Python to decrypt:
```python
from functools import reduce

shares = {i+1:int(l,16) for i,l in enumerate(open("cats.txt")) if l.strip()}
p = int("010000000000000000000000000000000000000000000000000000000000000129",16)
modinv = lambda a,p: pow(a,-1,p)

secret = sum(y * reduce(lambda x,j:x*-j%p,(j for j in shares if j!=i),1) * modinv(reduce(lambda x,j:x*(i-j)%p,(j for j in shares if j!=i),1),p) for i,y in shares.items()) % p
print(hex(secret), secret.to_bytes((secret.bit_length()+7)//8,"big"))
```

Ran python program on txt file:
```bash
python3 sss.py
0x2a5a49502070617373776f72643a20464170656b4a21794a363959616a5773 b'*ZIP password: FApekJ!yJ69YajWs'
```

Used the password `FApekJ!yJ69YajWs` to open the Zip.

Inside was a file called cute-kitty-noises.txt, which contained `Meow Lang`. Which looks like:
```
MeowMeow;MeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeowMeow
```

Found Meow Lang script:
```python
from pathlib import Path

decoded = ''.join(
    chr(s.count('Meow'))
    for s in Path('cute-kitty-noises.txt').read_text().replace(';;',';').split(';')
    if 32 <= s.count('Meow') <= 126
)

print(decoded
```

Decoded Meow Lang:
```python
python3 meow.py                                           
malware is illegal and for nerdscats are cool and badassflag{35dcba13033459ca799ae2d990d33dd3}
```


## Flag
`flag{35dcba13033459ca799ae2d990d33dd3}`


<p align="right">(<a href="#top">back to top</a>)</p>

---
title: "StegoRSA"
date: 2026-03-28
categories:
  - PicoCTF
tags:
  - "Cryptography"
  - "2026"
  - "RSA"
  - "Steganography"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/crypto.png
toc: true
toc_sticky: true
---

# 🔐 StegoRSA

|Category         |	Author                |
|-----------------|-----------------------|
|🔐 Cryptography         |Yahaya Meddy        |

## Challenge Prompt

A message has been encrypted using RSA. The public key is gone… but someone might have been careless with the private key. Can you recover it and decrypt the message?

Download the [flag](https://challenge-files.picoctf.net/c_plain_mesa/e2edf6e5f7186b3bbd28c265faf03846380e7ba3f0c7b3c7daf9193da3acb790/flag.enc) and [image](https://challenge-files.picoctf.net/c_plain_mesa/e2edf6e5f7186b3bbd28c265faf03846380e7ba3f0c7b3c7daf9193da3acb790/image.jpg).

## Problem Type
- RSA
- Steganography 

## Solve
We are given an image of a key and lock.<br>
![image](https://github.com/user-attachments/assets/521dce2a-2bb9-49fb-8fb3-2bf08f05b4f5)

If we run that in [Aperi'Solve](https://www.aperisolve.com/):<br>
<img width="1348" height="645" alt="2026-03-28_19-59-02" src="https://github.com/user-attachments/assets/19fb8d6b-0328-4cd1-a417-6e1e2ac4e07c" />

Down in the Identify section we see some hex in the "Comment" field:<br>
<img width="1395" height="399" alt="2026-03-28_19-59-44" src="https://github.com/user-attachments/assets/2d2eef0a-ce81-418e-ab78-18fd63e47c2e" />

If we run that hex through [CyberChef](https://gchq.github.io/CyberChef) we get a Private Key:<br>
<img width="1534" height="889" alt="2026-03-28_20-00-25" src="https://github.com/user-attachments/assets/9e07f1e2-3060-44cc-9276-fedae42a7abb" />

Save the private key in a new text file called `key.pem`.

Then we can use OpenSSL to decyrpt:<br>
```bash
openssl pkeyutl -decrypt -in flag.enc -out flag.txt -inkey key.pem
```

That will output the flag to a new file called `flag.txt`:<br>
<img width="979" height="193" alt="image" src="https://github.com/user-attachments/assets/f6c72cfe-2245-4289-8c41-89baadac4dfa" />

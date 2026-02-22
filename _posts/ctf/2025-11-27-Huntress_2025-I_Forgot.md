---
title: "I Forgot"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "Forensics"
  - "2025"
  - "Memory Forensics"
platform: Huntress 2025
competition_year: 2025
header:
  teaser: /assets/images/teasers/forensics.png
toc: true
toc_sticky: true
---

# 🔍 I Forgot

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics     |John Hammond           |

## Challenge Prompt

So.... bad news.

We got hit with ransomware.

And... worse news... we paid the ransom.

After the breach we FINALLY set up some sort of backup solution... it's not that good, but, it might save our bacon... because my VM crashed while I was trying to decrypt everything.

And perhaps the worst news... I forgot the decryption key.

Gosh, I have such bad memory!!

## Password
The archive password is `i_forgot`.
{: .notice--primary}


## Problem Type
- Memory Forensics

## Solve
Started with checking processes:
`vol -f memdump.dmp windows.pstree`

Saw that PowerShell was run and it spawned BackupHelper and Notepad.

Dumped the BackupHelp PID:
`vol -f memdump.dmp windows.memmap --dump --pid 2132`

Ran Binwalk on that:
```bash
Binwalk pid.2132.dmp
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
16384         0x4000          Zip archive data, encrypted at least v2.0 to extract, compressed size: 1324, uncompressed size: 1708, name: private.pem
17793         0x4581          Zip archive data, encrypted at least v1.0 to extract, compressed size: 268, uncompressed size: 256, name: key.enc
18300         0x477C          End of Zip archive, footer length: 22
```

Saw the first files were a zip with a `.pem` and `.enc` file. Extracted those:
```bash
dd if=pid.2132.dmp of=first_file.zip bs=1 skip=16384 count=1916
```
`if` = in file<br>
`of` = out file<br>
`bs` = block size of 1 byte<br>
`skip` = Start in decimal of file<br>
`count` = How big the file is in bytes (16384 - 18300 = 1916)

The zip required a password, so I ran `strings` on the full memdump:
```bash
strings memdump.dmp | egrep -iE "private.pem" -A 3 -B 3
```

Found this that was cut off:
```
ing 7-Zip (GUI): Right-click -> Open archive -> enter password ePDaACdOCwaMiYDG
2) Decrypt the AES key+IV:
   openssl pkeyutl -decrypt -inkey private.pem -in key.enc -out key_raw.bin -pkeyopt rsa_p
MDECRYPT_PRIVATE_KEY.zip
```

Used the password `ePDaACdOCwaMiYDG` to extract the zip.

I had to figure out the missing bits from the strings extraction using Google, but:
```bash
openssl pkeyutl -decrypt -inkey private.pem -in key.enc -out key_raw.bin -pkeyopt rsa_padding_mode:oaep
```

Add the key_raw to CyberChef
- To Hex
```
28 9e a5 8a 38 54 9d 5f af 7a 97 a6 dd 19 cd f2 dd c0 49 6a 8a 64 f9 9a 77 c6 43 52 9c 94 b8 04 
2c 6a 55 b0 a8 91 41 05 65 17 68 7a 97 73 05 d6
```

Added Flag.enc to CyberChef
- AES Decrypt
- First line as Hex Key
- Second line as Hex IV
- Mode CBC
- Input and Output both Raw

```
=== CONFIDENTIAL RECOVERY ===
Note: This file contains the recovered data for decryption.
-----------------------------
FLAG:
flag{fa838fa9823e5d612b25001740faca31}
-----------------------------
```

## Flag
`flag{fa838fa9823e5d612b25001740faca31}`


<p align="right">(<a href="#top">back to top</a>)</p>

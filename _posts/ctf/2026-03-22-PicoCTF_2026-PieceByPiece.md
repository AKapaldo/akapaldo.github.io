---
title: "PieceByPiece"
date: 2026-03-22
categories:
  - PicoCTF
tags:
  - "Miscellaneous"
  - "2026"
  - "Zip File"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 📦 Piece by Piece

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous         |Yahaya Meddy       |

## Challenge Prompt

After logging in, you will find multiple file parts in your home directory. These parts need to be combined and extracted to reveal the flag.

## Problem Type
- Zip File


## Solve
We are told we need to SSH into the machine, so we start with that.
<img width="856" height="479" alt="2026-03-22_13-25-06" src="https://github.com/user-attachments/assets/b068fce9-1dca-4e22-b0a0-9e9c60c43e95" />

Check the folder and see we have `instructions.txt` and `part_aa` through `part_ae`.
<img width="572" height="204" alt="2026-03-22_13-25-19" src="https://github.com/user-attachments/assets/2c43b285-4d50-453f-970d-fc8357ad3884" />

The instructions:
- The flag is split into multiple parts as a zipped file.
- Use Linux commands to combine the parts into one file.
- The zip file is password protected. Use this "supersecret" password to extract the zip file.
- After unzipping, check the extracted text file for the flag.
{: .notice--info}


So, let's combine all the files together into `part_aa` by appending them to the end in order.
<img width="412" height="87" alt="2026-03-22_13-26-15" src="https://github.com/user-attachments/assets/1b9bb922-0bd1-49d6-8bd0-449e8f96e7cf" />

Then we can rename the file to `flag.zip`.<br>
<img width="363" height="38" alt="2026-03-22_13-26-37" src="https://github.com/user-attachments/assets/8eb7b5c0-b1e1-4d52-a568-edf22f5130d4" />

Then unzip the file, using `supersecret` as the password:<br>
<img width="398" height="80" alt="2026-03-22_13-27-27" src="https://github.com/user-attachments/assets/23bd9318-f7d8-4eb8-86b2-095aa21daf12" />

And finally open the file to get the flag!<br>
<img width="567" height="43" alt="2026-03-22_13-27-41" src="https://github.com/user-attachments/assets/88626740-5c83-497c-bf73-d0b4f0efc3b0" />


---
title: "Angler"
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

# 📦 Angler

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |Tim Sword              |

## Challenge Prompt

These scribbles are impossible to read!

`42 6c 6f 77 66 69 73 68`
Some crazy fisherman came by, dropped this note, and was muttering something in his drunken stupor, about his fishing pole
and taking out... murlocs in Entra? and CyberChef!?

I don't get it. You're the expert here! Not me!

## Problem Type
- Azure Enumeration

## Solve 1

Added the scribbles.dat file to CyberChef
From Blowfish
- Key: Blowfish
- IV first 8 Bytes (16 chars) and remove them from the input

Creates binary (had to remove the first set… not sure why?) - `00110011`
- Remove Whitespeace
- From Binary
- Remove Whitespace
- Reverse (byte)
- From Hex
- From Base64

```
phisher@4rhdc6.onmicrosoft.com:PhishingAllTheTime19273!!

My sea is made of data, my shore a glowing screen, I cast my line with careful code, in a vast and global scene.
My lure is not a worm or fly, but a name you trust, a prize, My hook is hid within a link, disguised before your eyes.
I do not fish for flesh or fin, but for a private key, reeling in the secrets you mistakenly give to me.
Send to that which is found within the above, to the destination you have yet to reveal, and the secret you seek will reveal.
```

Login to the [M365 Admin Console](Admin.microsoft.com) with creds above.

Groups > Active Group > Security Groups

## Flag 1
`flag{mczxals2amxc}`

## Solve 2
```powershell
Get-EntraUser -All | ConvertTo-Json -Depth 6
```

## Flag 2
`flag{02818nccnasd}`


## Solve 3 & 4
```powershell
Get-MgServicePrincipal -All -Property * | ConvertTo-Json -Depth 6
```

## Flag 3 & 4
`flag{2naxajsmcwijdm}`

`flag{3mcnzxjaslwinca}`

## Solve 5
Ran a search in Purview search. This would have got all but the final "?" flag:

## Flag 5
`flag{928nzlasdu2}`

## Solve 6
Found this email address where the domain didn't match any others:
nattyp@51tjxh.onmicrosoft.com

Sent an email with "Blowfish" as the subject and got this back:
```
Congratulations! Here's your final flag: flag{didsomeonesay..?}


I hope you enjoyed the challenge. Go check out the others!


There are optional bonus flags hidden in the Entra environment that contain this source email address. Grab them and claim bonus points!


[Cheers.](https://wowpedia.fandom.com/wiki/Nat_Pagle)
```


## Flag 6
`flag{didsomeonesay..?}`



<p align="right">(<a href="#top">back to top</a>)</p>

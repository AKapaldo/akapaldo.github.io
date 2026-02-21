---
title: "Webshellz"
date: 2025-11-26
categories:
  - Huntress_2025
tags:
  - "Forensics"
  - "2025"
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 🔍 Webshellz

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics     |Ben Folland            |

## Challenge Prompt

The sysadmin reported that some unexpected files were being uploaded to the file system of their IIS servers.

As a security analyst, you have been tasked with reviewing the Sysmon, HTTP, and network traffic logs to help us identify the flags!

## Problem Type
- PCAP
- EVTX File (Windows Event Log)

## Password
> [!NOTE]
> The password to the ZIP archive is `webshellz`.

## Solve 1

Open Wireshark:
Filter to `http` or `frame contains "ZmxhZ3"`
packet 502
- Right click
- Show Packet Bytes
- Decode as Base64

## Flag 1
`flag{fb4e078a739ac4ce687eb78c2e51aafe}`

## Solve 2
Filter to `frame contains "MZWGCZ33"`
Packet 19370
Paste into CyberChef<br>
From Base32

## Flag 2
`flag{c7ba76c0a4484fe8c135a1195e8d94ed}`

## Solve 3
In the evtx file:
`C:\Windows\system32\net1  user IIS_USER VJGSuERc6qYAYPdRc556JTHqxqWwLbPwzABc0XgIhgwYEWdQji1 /add`
Paste into CyberChef<br>
From Base62

## Flag 3
`flag{03638631595684f0c8c461c24b0879e6}`

## Resources
- [Online EVTX File Reader](https://omerbenamram.github.io/evtx/)


<p align="right">(<a href="#top">back to top</a>)</p>

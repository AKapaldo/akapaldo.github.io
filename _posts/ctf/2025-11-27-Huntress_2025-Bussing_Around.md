---
title: "Bussing Around"
date: 2025-11-26
categories:
  - Huntress_2025
tags:
  - Forensics
  - 2025
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 🔍 Bussing Around

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics          |Soups71          |

## Challenge Prompt

One of the engineers noticed that an HMI was going haywire.

He took a packet capture of some of the traffic but he can't make any sense of it... it just looks like gibberish!

For some reason, some of the traffic seems to be coming from someone's computer. Can you help us figure out what's going on?

## Problem Type
- PCAP
- Modbus
- Industrial Control System (ICS)

## Solve
Filter on Modbus, unit 38, source port 502
```wireshark
((modbus) && (mbtcp.unit_id == 38)) && (tcp.srcport == 502)
```
File > Export Packet Disections > As Plain Text with just the Modbus panel expanded

Import txt file to CyberChef:
- Regex `.*Register Value \(UINT16\)\:.*`
- Find/Replace: Find `        Register Value (UINT16): ` and Replace with nothing
- Remove Whitespace
- From Binary

Text shows:
`The password is 5939f3ec9d820f23df20948af09a5682`

- Add extract files
- Export zip

Open Zip with the password above.

## Flag
`flag{8c8e0e59d1292298b64c625b401e8cfa}`


<p align="right">(<a href="#top">back to top</a>)</p>

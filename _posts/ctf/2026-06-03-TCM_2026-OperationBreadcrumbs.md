---
title: "OperationBreadcrumbs"
date: 2026-06-03
categories:
  - TCM
tags:
  - "Web"
  - "2026"
  - "Web"
  - "OSINT"
  - "Cryptography"
platform: TCM 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/web.png
toc: true
toc_sticky: true
---

# 🌐 Operation Breadcrumbs

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web      |The Cyber Mentor    |

## Challenge Prompt

Operation Breadcrumbs

Welcome, operator. Your flag is prepared on demand, straight from the TCM flag service.

Request your drop with the button below. When it arrives, submit it in the field underneath.

Flags follow the format TCM{...}. Good luck!

## Problem Type
- Web
- OSINT
- Cryptography

## Solve
We start out with a page showing a `Download Flag` button, but when we click it, we get a toast notification with:
```
⚠️ Something went wrong

The flag service is temporarily unavailable. Our team has been notified.
```

<img width="1912" height="911" alt="2026-05-27_19-34-23" src="https://github.com/user-attachments/assets/ca077686-7033-4924-a503-dc94ff958772" />

If we click the button with the Developer Tools open (F12), under the console tab, we see:
`XHR GET https://ctf.tcmsecurity.com/api/flag`

<img width="1911" height="694" alt="2026-05-27_19-34-57" src="https://github.com/user-attachments/assets/32b1064d-7392-4420-9c72-1eff58f6bf73" />


If we expand that, under the response headers there is a `x-debug-trace` header with 
```
aHR0cHM6Ly9naXN0LmdpdGh1YnVzZXJjb250ZW50LmNvbS9NYWx3YXJlQ3ViZS9mYjA3NDM0YzFmYmEzYjkxNDNjYWU4ZjAxMzA5YTU3Zi9yYXcvNTk4MTRkMGY0MTU4MTMyNWJhZTVjODBiMGRlOTg0NDk2M2Q0NGI0Ny9mbGFnLXNlcnZpY2UtZGVidWctbm90ZXMubWQ=
```

<img width="1917" height="381" alt="2026-05-27_19-35-10" src="https://github.com/user-attachments/assets/6c8ea051-4695-43fc-9dea-0a878e28bd47" />


Also, the status returned is [418 (I'm a Teapot)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/418?utm_source=mozilla&utm_medium=devtools-netmonitor&utm_campaign=default).

If we put that into [CyberChef](https://gchq.github.io/CyberChef/) and use the `from Base64` recipe, we get:
```
https://gist.githubusercontent.com/MalwareCube/fb07434c1fba3b9143cae8f01309a57f/raw/59814d0f41581325bae5c80b0de9844963d44b47/flag-service-debug-notes.md
```

<img width="1530" height="307" alt="2026-05-27_19-35-49" src="https://github.com/user-attachments/assets/8c25809a-a7e8-40bd-9f4a-404b0f0349b9" />


Visiting that site gives us a note with:
```
# upload worker - queue stalls

worker keeps choking on the bulk import. the per-item debug dump sits on the
internal api:
/api/internal/ff9d9e38a38333145e46b49aa4a5f4b6?_=1718041920473&rid=7f3c9a2b1e&debug=1

grabbed that this morning, was useful. came back after lunch and it's 404.
of course it is. this is what I get for vibe-coding the whole thing...
  ```

If we attempt to go to the API link, as suggested in the note, we get a 404 error.

<img width="1919" height="270" alt="2026-05-27_19-36-48" src="https://github.com/user-attachments/assets/98300fac-3e65-4ea0-88d7-13709f78196d" />


Next I took a look at the API url:
`ff9d9e38a38333145e46b49aa4a5f4b6` this looked like a hash, so I went to [Crack Station](https://crackstation.net/) to check it.

<img width="1905" height="538" alt="2026-05-27_20-58-48" src="https://github.com/user-attachments/assets/e3f11dfa-c156-429b-b28d-ee4d1c65393e" />


This turned out to be a MD5 hash for `IMG27`. From there I checked the API against hashes from 1 to 100:
```bash
for i in $(seq 1 100); do hash=$(echo -n "IMG$i" | md5sum | cut -d' ' -f1); echo "IMG$i ($hash): "; curl -s "https://ctf.tcmsecurity.com/api/internal/$hash"; echo ""; sleep 1; done
```

All of those except IMG28 resulted in a 404:
```json
IMG28 (ce148ab4b8a20f6d0005775ad6320ceb): 
{"auth_payload":"H4sIAAAAAAAA/wTA7wqCMBAA8He5z6kthUiISiEiUIJGfz6JXtOGbRd6wzR6935fuHkyzTxJrbIQQ/05hdGlwcRMeXFY7cXiuJRVc72bfI6ijZKIREJTCDN46Z61bSCGJ/O7j4NgGAZ/JMeuUj6SCbYyzc4KXad53GH5UGbc9K4qkGytO1OyJrsW8PsHAAD//71EzlGFAAAA","service":"image-processor","status":"ok"}
```

Back to [CyberChef](https://gchq.github.io/CyberChef/) and this time I used the `magic` recipe to determine this was both `From Base64` and `Gunzip`:
```json
{"X-TCM-Token":"fxP34VgcBmzN_H9F12J7TbgWYmN0c1k4B4o1Boz3","listing":"https://www.youtube.com/@TCMSecurityAcademy?sub_confirmation=1"}
```

<img width="1530" height="291" alt="2026-05-27_21-00-53" src="https://github.com/user-attachments/assets/0c50691b-46c8-4b00-81bf-1544412fb783" />


I visited the [YouTube](https://www.youtube.com/@TCMSecurityAcademy) page and in the links near the header there was a link called `BREADCRUMB`.

<img width="558" height="720" alt="2026-05-30_21-27-49" src="https://github.com/user-attachments/assets/bd5258e9-f946-4153-a63d-7f0251206297" />


This link directed us to [https://ctf.tcmsecurity.com/tcm-prod-media/4ee5f8ff6d6a23deb9d829479b54c8e3.jpg](https://ctf.tcmsecurity.com/tcm-prod-media/4ee5f8ff6d6a23deb9d829479b54c8e3.jpg)

Which displayed an XML error page:
```xml
<Error>
<Code>AccessDenied</Code>
<Message>Access Denied</Message>
<RequestId>9F3A2C1D7E4B8A60</RequestId>
<HostId>
Uf3pK2mWqL8xY1nZ7bV4tR6sD0gH5jC9aE2oP1iM3kS8wB7vN4xQ6lT0yU2rA1c=
</HostId>
</Error>
```

<img width="927" height="271" alt="2026-05-30_21-28-35" src="https://github.com/user-attachments/assets/a344f148-4c42-49c6-8d09-230182bdd056" />


Next I used `curl` to send the `X-TCM-Token` header and used `-o` to download the file:
```sh
curl https://ctf.tcmsecurity.com/tcm-prod-media/4ee5f8ff6d6a23deb9d829479b54c8e3.jpg \
  -H "X-TCM-Token: fxP34VgcBmzN_H9F12J7TbgWYmN0c1k4B4o1Boz3" -o image.jpg
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100 627.4k   0 627.4k   0      0 650.0k      0                              0
```

Once I had the image I opened it up and saw that it was some leaves against a plain background:<br>

<img width="4032" height="3024" alt="image" src="https://github.com/user-attachments/assets/02768ff3-18f1-4129-b05b-d2dd957b960f" />

I ran `exiftool` against it and found it had coordinates in the output:
```sh
GPS Latitude                    : 34 deg 8' 2.76" N
GPS Longitude                   : 118 deg 19' 17.40" W
GPS Position                    : 34 deg 8' 2.76" N, 118 deg 19' 17.40" W
```

I put this location into [CalTopo](https://www.caltopo.com) and saw it was in Hollywood, CA:<br>
<img width="1430" height="704" alt="Screenshot 2026-06-03 at 6 17 53 AM" src="https://github.com/user-attachments/assets/fc77dcd6-ba35-4e48-9abd-152ffd2bfdaa" />


Next I checked to see if it was really a `JPG`, and it was not:
```sh
file image.jpg        
image.jpg: Zip archive, with extra data prepended
```

I renamed the file to a `ZIP`:
```sh
mv image.jpg image.zip 
```

Then I unzipped the file revealing `flag.xor`:
```sh
unzip image.zip                                                         
Archive:  image.zip
warning [image.zip]:  642481 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: flag.xor
```

Back to CyberChef and I added the `flag.xor` file and then used the XOR operation with the key `HOLLYWOOD` to reveal the flag: <br>
<img width="1685" height="573" alt="Screenshot 2026-06-03 at 6 24 29 AM" src="https://github.com/user-attachments/assets/f86bef38-baa6-4cbe-a872-cf13bde79513" />

<br>

<img width="1439" height="747" alt="2026-05-30_21-50-48" src="https://github.com/user-attachments/assets/061503df-5398-4313-ac15-c3120101ee5f" />


## Alterative Solve

Since we know the flag starts with `TCM{` we can try a known plaintext attack.
```python
with open('flag.xor', 'rb') as f:
    ciphertext = f.read()

known_plaintext = b"TCM{"
key_prefix = bytearray()

for i in range(len(known_plaintext)):
    key_prefix.append(ciphertext[i] ^ known_plaintext[i])

print(f"[*] The first 4 characters of the key are: {key_prefix}")
```

This gives us that the first 4 characters are `HOLL`. Then we can use the [Webster's Dictionary](https://www.merriam-webster.com/wordfinder/classic/begins/all/-1/holl/1) to find words that start with `HOLL`. We could also use a file like `rockyou.txt` to do this too. We pick some words in that list that we think might be the key:
```python
with open('flag.xor', 'rb') as f:
    ciphertext = f.read()

key = b"HOLL"
decrypted = bytearray([ciphertext[i] ^ key[i % len(key)] for i in range(len(ciphertext))])
print(f"[*] Testing exact 4-byte key 'HOLL': {decrypted}\n")

key = b"HOLLY"
decrypted = bytearray([ciphertext[i] ^ key[i % len(key)] for i in range(len(ciphertext))])
print(f"[*] Testing exact 4-byte key 'HOLL': {decrypted}\n")

key = b"HOLLYWOOD"
decrypted = bytearray([ciphertext[i] ^ key[i % len(key)] for i in range(len(ciphertext))])
print(f"[*] Testing exact 4-byte key 'HOLL': {decrypted}\n")

key = b"HOLLAND"
decrypted = bytearray([ciphertext[i] ^ key[i % len(key)] for i in range(len(ciphertext))])
print(f"[*] Testing exact 4-byte key 'HOLL': {decrypted}\n")

key = b"HOLLOW"
decrypted = bytearray([ciphertext[i] ^ key[i % len(key)] for i in range(len(ciphertext))])
print(f"[*] Testing exact 4-byte key 'HOLL': {decrypted}\n")
```


## Flag
`TCM{WH3R3_DR34M5_ARE_M4D3}`

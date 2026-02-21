---
title: "Trust Me"
date: 2025-11-26
categories:
  - Huntress_2025
tags:
  - "Miscellaneous"
  - "2025"
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 📦 Trust Me

|Category         |	Author                |
|-----------------|-----------------------|
|📦 Miscellaneous |John Hammond           |

## Challenge Prompt

C'mon bro, trust me! Just trust me!! Trust me bro!!!

The TrustMe.exe program on this Windows desktop "doesn't trust me?"

It says it will give me the flag, but only if I "have the permissions of Trusted Installer"...?

## Problem Type
- Windows Trusted Installer

## Solve
Set up python HTTP server on local 
* GUI version in folder
```python
import http.server
import socketserver

# Set the file you want to serve
file_to_serve = 'path/to/your/file.txt'

# Specify the port you want to use
port = 8000

# Define a custom request handler to disable logging
class SilentHandler(http.server.SimpleHTTPRequestHandler):
    def log_message(self, format, *args):
        pass

# Create the server
with socketserver.TCPServer(("", port), SilentHandler) as httpd:
    print(f"Serving {file_to_serve} at http://localhost:{port}")
    # Set the working directory to the folder containing the file
    httpd.directory = file_to_serve[:file_to_serve.rfind('/')]
    # Open the file and serve it
    httpd.serve_forever()
```

Connected to server/my machine (10.1.241.187:8000) and pulled off [Advanced Run Software](https://www.nirsoft.net/utils/advanced_run.html)
* Version saved in folder too

Ran program as TrustedInstaller

## Flag
`flag{c6065b1f12395d526595e62cf1f4d82a}`


<p align="right">(<a href="#top">back to top</a>)</p>





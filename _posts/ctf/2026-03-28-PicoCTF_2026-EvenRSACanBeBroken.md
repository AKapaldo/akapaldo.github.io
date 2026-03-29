---
title: "EvenRSACanBeBroken"
date: 2026-03-28
categories:
  - PicoCTF
tags:
  - "CTF"
  - "2026"
  - "RSA"
platform: PicoCTF 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/ctf.png
toc: true
toc_sticky: true
---

# 🔒 Even RSA Can Be Broken???

|Category         |	Author                |
|-----------------|-----------------------|
|🔒 Cryptography         |Michael Crotty        |

## Challenge Prompt

This service provides you an encrypted flag. Can you decrypt it with just N & e?

## Problem Type
- RSA

## Solve
We are told we need to nc into the machine, so we start with that.
<img width="1329" height="114" alt="2026-03-28_19-07-14" src="https://github.com/user-attachments/assets/badf7618-5d81-4d43-ae30-799af7f1968f" />

When we do that we get 3 numbers: `N`, `e`, and the cyphertext. As the challenge name suggests, this is RSA encryption.

What is interesting here is that `N` is normally 2 large random primes multiplied together `p` and `q`. This will always result in an odd number.

Unless... one of the primes is 2. If we look at our `N` we can see it is an even number and with that we can break this.

You could also use [FactoDB](https://factordb.com/index.php) to figure out `p` and `q`:
<img width="1905" height="391" alt="2026-03-28_19-42-10" src="https://github.com/user-attachments/assets/b1f02442-edfd-4b94-af14-bc1cf2e685ec" />

So we can solve this with some Python:
```python
# Import module depending on version this might be Crypto instead of Cryptodome for you.
from Cryptodome.Util.number import long_to_bytes

# We are given e, N, and our cyphertext in our output from the connection
e = 65537
N = 16830092949573620562109923004380422032555253605649015077259977439249981036204623428721679013333135864661234780499132068466855125435572472785173392998098638
cyphertext = 6753492921406791814444560806732404564578446163893060665321387843570118001023098154052060062257197953137887777264301927898679921962615194346118110890025225

# Now we know either p or q is 2 beacuse our N is even, so let's assign 2 to p
p = 2

# Now we can solve for q
q = N // p

# Now we can calculate Euler's totient
phi = (p - 1) * (q - 1)

# The modular inverse to get the private key d.
# You can also do this by importing inverse with long_to_bytes above and running d = inverse(e, phi)
d = pow(e, -1, phi)

# Decrypt to positive integer
plain = pow(cyphertext, d, N)

# Convert a positive integer to a byte string using big endian encoding
print(long_to_bytes(plain))
```
We run all those pieces in Python and we get the flag!
<img width="1322" height="278" alt="2026-03-28_19-30-23" src="https://github.com/user-attachments/assets/ab06c754-92e8-44ca-bf97-9feb23ab790e" />

The final step can be done in [CyberChef](https://gchq.github.io/CyberChef) as well if you don't have the Crypto or Cryptodome packages:
<img width="1530" height="286" alt="2026-03-28_19-26-52" src="https://github.com/user-attachments/assets/c3fedc3b-7bdb-4703-959f-5e1f9d142829" />


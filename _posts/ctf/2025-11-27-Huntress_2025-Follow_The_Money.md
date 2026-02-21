---
title: "Follow The Money"
date: 2025-11-26
categories:
  - Huntress
tags:
  - "OSINT"
  - "2025"
  - "OSINT"
platform: Huntress 2025
competition_year: 2025
toc: true
toc_sticky: true
---

# 🕵️ Follow The Money

|Category         |	Author                |
|-----------------|-----------------------|
|🕵️ OSINT          |Brady         |

## Challenge Prompt

Hey Support Team,

We had a bit of an issue yesterday that I need you to look into ASAP. There's been a possible case of money fraud involving our client, Harbor Line Bank. They handle a lot of transfers for real estate down payments, but the most recent one doesn't appear to have gone through correctly.

Here's the deal, we need to figure out what happened and where the money might have gone. The titling company is looping in their incident response firm to investigate from their end. I need you to quietly review things on our end and see what you can find. Keep it discreet and be passive.

I let Evelyn over at Harbor Line know that someone from our team might reach out. Her main email is offline right now just in case it was compromised, she's using a temporary address until things get sorted out:

`evelyn.carter@51tjxh.onmicrosoft.com`

## Problem Type
- OSINT

## Password
The password to the ZIP archive below is `follow_the_money`.
{: .notice--primary}


## Solve
Zip file has a bunch of emails. Opened each and reviewed.<br>
Last email has a different link:
[https://evergatetltle.netlify.app](https://evergatetltle.netlify.app)

After clicking the `Transfer Closing Funds` button and submitting data gives B64 sting:
`aHR0cHM6Ly9uMHRydXN0eC1ibG9nLm5ldGxpZnkuYXBwLw==`

Decodes to:
[https://n0trustx-blog.netlify.app/](https://n0trustx-blog.netlify.app/)

Hacker name is `N0trustX`.


Visit GitHub<br>
Open `spectre.html` repo<br>
Part way down in the HTML is 
```html
<div id="encodedPayload" class="hidden">ZmxhZ3trbDF6a2xqaTJkeWNxZWRqNmVmNnltbHJzZjE4MGQwZn0=</div>
```


## Flag
`flag{kl1zklji2dycqedj6ef6ymlrsf180d0f}`


<p align="right">(<a href="#top">back to top</a>)</p>

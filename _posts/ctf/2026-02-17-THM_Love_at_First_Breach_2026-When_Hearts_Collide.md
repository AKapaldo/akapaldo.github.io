---
title: "When Hearts Collide"
date: 2026-02-17
categories:
  - THM_Love_at_First_Breach_2026
tags:
  - "Web"
        - "2026"
        - "MD5 Collision"
platform: THM Love at First Breach 2026
competition_year: 2026
toc: true
toc_sticky: true
---

# 🌐 Love at First Breach 2026 - When Hearts Collide

|Category         |	Author                |
|-----------------|-----------------------|
|🌐 Web         |TryHackMe      |

## Challenge Prompt
My Dearest Hacker,

Matchmaker is a playful, hash-powered experience that pairs you with your ideal dog by comparing MD5 fingerprints. Upload a photo, let the hash chemistry do its thing, and watch the site reveal whether your vibe already matches one of our curated pups. The algorithm is completely transparent, making every match feel like a wink from fate instead of random swipes.

Come get your dog today!

You can access the web app here: `http://MACHINE_IP`

## Problem Type
- MD5 Collision

## Solve
When we load into the page, we are presented with Matchmaker, which will pair us up with our perfect dog based on MD5 hash:
<img width="749" height="806" alt="2026-02-15_14-06-51" src="https://github.com/user-attachments/assets/041d7885-a5c0-4be6-969d-574add270d64" />


MD5 has known collisons and the title "When Hearts Collide" suggests we are going to be making an intentional collision.<br>
The instructions say if our hash is identical to a dog's we will be matched:
<img width="589" height="395" alt="2026-02-15_14-08-40" src="https://github.com/user-attachments/assets/cb07e3a4-fdae-4878-8fa9-d9d282d8c18d" />


Let's click the "browse a random match" link so we can get a file to hash.<br>
Here we are presented with an image of a dog. We will right click and save that image to our machine. Just to note, you don't have to use this picture, you can use any picture. <br>
<img width="1268" height="877" alt="2026-02-15_14-07-42" src="https://github.com/user-attachments/assets/94dc8b4e-d39b-453d-a706-fdb33961aaf7" />


I used the [FastColl](https://github.com/brimstone/fastcoll) tool on GitHub to make my collision.
We can run the following command in the terminal to run the tool in Docker:
```bash
docker run --rm -it -v $PWD:/work -w /work -u $UID:$GID brimstone/fastcoll --prefixfile dog_from_site.jpg -o file1.jpg file2.jpg
```

Let's break that down real quick:<br>
`docker run` - Starts a new container.<br>
`--rm` - Automatically deletes the container after it exits (no leftover container).<br>
`-it` -i = interactive -t = allocate terminal (Allows you see and interact with output.) <br>
`-v $PWD:/work` - Mounts your current directory into the container at /work. Files in your current folder are visible inside the container. Any files created inside /work appear in your real directory. <br>
`-w /work` - Sets the working directory inside the container to /work.<br>
`-u $UID:$GID` - Runs the container as your user ID and group ID. This prevents output files from being owned by root.<br>
`brimstone/fastcoll` - A Docker image containing FastColl, an MD5 collision generator.<br>
`--prefixfile dog_from_site.jpg` - Uses dog_from_site.jpg as a chosen prefix. The generated collision files will start with the exact contents of dog_from_site.jpg. The collision blocks will be appended after it.<br>
`-o file1.jpg file2.jpg` - Specifies the two output files: file1.jpg & file2.jpg. These two files will both begin with the exact contents of dog_from_site.jpg and differ after that prefix but have the same MD5 hash.<br>

Then we can see our output files both have the same MD5 Hash:
<img width="1350" height="414" alt="2026-02-15_14-37-59" src="https://github.com/user-attachments/assets/299467a1-fc19-4274-979d-61ee0d578713" />


First, we submit `file1.jpg` to get it in the database and we will be told there is no match:
<img width="671" height="774" alt="2026-02-15_15-04-31" src="https://github.com/user-attachments/assets/69af1ce8-dfb3-4699-95bf-f74f69f49166" />


Then, if we submit `file2.jpg`, it will match `file1.jpg` and we get the flag:
<img width="657" height="963" alt="2026-02-15_14-25-57" src="https://github.com/user-attachments/assets/99b9a62b-1786-469c-b2c7-2b80f338fe6d" />


Here is another example using a cat picture instead following the same steps:
<img width="663" height="965" alt="2026-02-15_15-07-42" src="https://github.com/user-attachments/assets/8bc9a46b-c667-4c90-8cd7-f18fb8e90136" />


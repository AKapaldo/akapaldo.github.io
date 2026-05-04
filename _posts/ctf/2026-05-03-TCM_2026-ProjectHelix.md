---
title: "ProjectHelix"
date: 2026-05-03
categories:
  - TCM
tags:
  - "Forensics"
  - "2026"
  - "Audio"
  - "Forensics"
  - "Web"
platform: TCM 2026
competition_year: 2026
header:
  teaser: /assets/images/teasers/forensics.png
toc: true
toc_sticky: true
---

# 🔍 Project Helix

|Category         |	Author                |
|-----------------|-----------------------|
|🔍 Forensics       |The Cyber Mentor    |

## Challenge Prompt

Project Helix: CTF Scenario

Three days ago, lead geneticist Dr. Owens sent a final frantic transmission claiming he had intercepted a "biological broadcast." He described it as a repeating signal he believed to be a synthetic RNA sequence manifesting as radio frequency interference.

In violation of Protocol 4, Owens began unauthorized testing on a high-risk specimen. All communications stopped soon after. A security sweep of Owens' laboratory confirmed he is missing. His local workstation was found powered on, but all of his research has been deleted. Internal sensors indicate the "broadcast" is still active on a localized frequency, but the specific coordinates were deleted along with Owens' research notes.

You have been provided a forensic triage image of Owens' workstation. Your objective is to recover the deleted notes and identify the RNA-based key required to unlock his encrypted specimen archive.

## Problem Type
- Audio
- Forensics
- Web

## Solve
We start out by downloading a file called `2026-02-25T224057_Triage.zip`. When we extract that file, we are given a disk copy of a Windows C: drive.

I started out by exploring the `C:\Users` folder and found a `drowens` folder in there matching the Dr. Owens from our scenario.

In there looking in the `C:\users\drowens\AppData\Roaming\Microsoft\Windows\Recent` folder I saw a shortcut to a file called `freq.txt`

I right-clicked to review the properties of the file:<br>
<img width="405" height="509" alt="2026-03-17_20-05-49" src="https://github.com/user-attachments/assets/80e84a80-2525-4c71-8162-31de591f7d86" />

This shortcut points to `C:\users\drowens\Downloads`, but we don't have that folder in what we were given so this must be a deleted file.

Next I used [Eric Zimmermans](https://ericzimmerman.github.io/) tools, specifically his MFT Explorer tool.

I opened the program and went to File > Load MFT and loaded the MFT from `C:\$MFT`. The process to load the file does take a while. Once it was loaded, I did a search for the `freq` file I saw earlier:<br>
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/6d8d6ab9-b542-4be2-89fe-8089b48ac8b8" />

Looking in the details of the file, I saw the Zone Identifier, what Microsoft Uses for the Mark of the Web the use for Downloaded files. This also contains the host URL, `https://ctf.tcmsecurity.com/3c7d7997..a7/freq.txt`.
As you can see, however, 2 of the bytes were bad, filling in any letters will give you a website so I needed to figure out which combo resulted in a different size than the default landing page.
Based on the other characters, I assumed it was just hex letters and numbers so I wrote a quick PowerShell script to get the page sizes. I knew `00` wasn't the right combo beacuse I manually visited that page, so I looked for any page not that size:
<img width="1096" height="60" alt="image" src="https://github.com/user-attachments/assets/e0abe21f-3f99-428e-b663-015716914392" />

```PowerShell
$wrong = ((Invoke-WebRequest -UseBasicParsing -Uri "https://ctf.tcmsecurity.com/3c7d799700a7/freq.txt").Content.Length / 1KB)

0..0xFF | ForEach-Object {
$hex = "{0:x2}" -f $_
$hex = $hex + "a7"
$size = ((Invoke-WebRequest -UseBasicParsing -Uri "https://ctf.tcmsecurity.com/3c7d7997$hex/freq.txt").Content.Length / 1KB)
  if ($size -ne $wrong) {
  write-host "Found: $hex   Size: $size"
  }
}
```
This gave me the result of `c1a7`:<br>
<img width="1092" height="191" alt="2026-05-03_17-59-39" src="https://github.com/user-attachments/assets/75f1d675-d904-480a-a829-20c5d08d8e65" />

{: .notice--info}

><strong>Alternative method:</strong><br>
> We can `grep` on the part that we know is good in the MFT, `3c7d7997` using `-a` to treat the file as text, `-b` to get the byte offset, and `-o` to only print the pattern.<br>
> MFT records are exactly 1024 bytes long, so they always start at multiples of 1024. The byte offset from above is in the middle of a record, so we use floor division to give us a whole number for `212669942 // 1024`.
> Then we multiply by 1024 to find the starting byte of the MFT record to inspect, giving us `212669440`.<br>
> Now we can use `dd` to find the data. `if=` for the input file, `bs=1` to set the block size to 1 byte, `skip=212669440` to start at the boundry we calculated, `count=100` to read the first 100 bytes after, and the `| xxd` to convert the binary to a hex dump we can read.<br>
><img width="666" height="385" alt="2026-03-17_21-38-23" src="https://github.com/user-attachments/assets/ef7e698e-f599-426e-8694-b551570e48fd" />

When we visit that page, we see the log file that was deleted:<br>
<img width="1913" height="979" alt="2026-03-17_21-35-55" src="https://github.com/user-attachments/assets/dda7b1ed-e46e-4314-8109-d6500ff90ac5" />

The file has a bunch of frequencies that are then followed with `(X)`. So I wanted to see if any were different. Using `grep -v "(X)" freq.txt` I found 1 different, `3817.3`:<br>
<img width="1350" height="559" alt="2026-03-17_21-41-34" src="https://github.com/user-attachments/assets/01667d04-8aa5-4a9f-a59d-c2f541864447" />

Removing the `freq.txt` from the web address takes you to a radio with `volume`, `RF gain`, `zoom`, and `radio freq` dials.:<br>
<img width="1344" height="859" alt="2026-05-03_18-35-07" src="https://github.com/user-attachments/assets/29b44b70-37b3-4ff5-85e3-051008d52e78" />

The `radio freq` dial doesn't seem moveable to the right frequency, but if you open dev tools and then look in the console and turn the radio on there is a hint:<br>
<img width="1919" height="388" alt="2026-03-20_06-41-18" src="https://github.com/user-attachments/assets/7c874b7f-969b-432c-9439-f4a9e5a41126" />

Following the instructions, we can set the dial to `3817.3`:<br>
<img width="1914" height="297" alt="2026-03-20_06-45-22" src="https://github.com/user-attachments/assets/2a2e6c60-2d82-4257-81a8-f421acf2df4d" />

Then we start to hear beeps over our speakers:<br>
<img width="787" height="530" alt="2026-03-20_06-43-36" src="https://github.com/user-attachments/assets/43cd485c-e3a1-4e5e-a3b1-773e0af2923e" />

We need to record the beeps for about 156 seconds to get 2 full 78 second cycles (we likely started in the middle of one) to make sure we have a full start to finish cycle.

This outputs a WEBM file. I used [Cloud Convert](https://cloudconvert.com/webm-to-wav) to convert the file to a WAV file instead.:<br>
<img width="1260" height="901" alt="2026-05-03_18-49-50" src="https://github.com/user-attachments/assets/6b7097a2-b351-4121-a714-bccee93ab4c5" />

Then I used Python and Matplotlib to analyze the beeps:
```python
import matplotlib.pyplot as plt
from scipy.io import wavfile

# Load the file
sample_rate, data = wavfile.read('SIG_INT_1774003543049.wav')

# If it's stereo, take one channel
if len(data.shape) > 1:
    data = data[:, 0]

# Create a Spectrogram
plt.figure(figsize=(12, 6))
plt.specgram(data, Fs=sample_rate, NFFT=1024, noverlap=512, cmap='viridis')

plt.title('Waterfall Analysis (Spectrogram)')
plt.ylabel('Frequency [Hz]')
plt.xlabel('Time [sec]')
plt.colorbar(label='Intensity [dB]')
plt.show()
```

This showed the beeps in a Spectrogram: <br>
<img width="1202" height="674" alt="2026-05-03_18-56-38" src="https://github.com/user-attachments/assets/9b75d5e8-79d8-44d2-a379-6a4231482cd3" />

Then I used the magnifying glass/zoom tool to see what frequencies we were using:
<img width="1202" height="674" alt="2026-05-03_18-56-49" src="https://github.com/user-attachments/assets/cb5c5e03-76e2-4724-8350-9d247354e352" />

This showed our beeps were at 1000, 2000, 3000, and 4000 Hz. Using the info from `LOG_088` in the `freq.txt` file we can map the letters to frequencies:
```
LOG_088:
It speaks in strands. A four-fold tongue. I saw it on the waterfall today. tHEY are sending a message... quaternary? every pulse is part of a base... A...C...G...U... but what is the charcode? They called me CRAZY!

If I start counting from 0 it starts to make sense.
```

So we make A=0, C=1, G=2, and U=3 or A=1000, C=2000, G=3000, and U=4000.

Now we can use Python to turn that into letters:
```python
import numpy as np
from scipy.io import wavfile

# 1. Load the WAV file
sample_rate, data = wavfile.read('SIG_INT_1774003543049.wav')
if len(data.shape) > 1: data = data[:, 0] # Use one channel if stereo

# 2. Define frequencies and their RNA mappings
# Based on LOG_088: A=0, C=1, G=2, U=3
freq_map = {1000: 'A', 2000: 'U', 3000: 'G', 4000: 'C'}
targets = np.array([1000, 2000, 3000, 4000])

# 3. Analyze chunks (Adjust 'chunk_size' if the output looks too fast or slow)
chunk_size = int(sample_rate * 0.1) # 0.1 seconds per pulse
sequence = []

for i in range(0, len(data), chunk_size):
    chunk = data[i:i+chunk_size]
    if len(chunk) < chunk_size: break
    
    # FFT to find the loudest frequency in this chunk
    fft_data = np.abs(np.fft.rfft(chunk))
    freqs = np.fft.rfftfreq(len(chunk), 1/sample_rate)
    
    # Find which target frequency is loudest
    peak_idx = np.argmax(fft_data)
    peak_freq = freqs[peak_idx]
    
    # Match to the closest target (1k, 2k, 3k, or 4k)
    closest_freq = targets[np.argmin(np.abs(targets - peak_freq))]
    sequence.append(freq_map[closest_freq])

# 4. Print the raw RNA string
full_string = "".join(sequence)
print(f"Detected RNA Sequence: {full_string}")
```
This gives us:
```
AAAAAAAACCCCCCCCCGCCCACUUUUUUUUUGGGGGGGGGCCCCCCCCAAAAAAAAACCCACUCUUUUUUUUUGGGGGGGGGGGGGGGGUUUUUUUUUCCACUCCUUUUUUUUUCCCCCCCCAAAAAAAAAAAAAAAAACACUCCCUUUUUUUUUCCCCCCCCCCCCCCCCAAAAAAAAAACUCCCGUUUUUUUUUCCCCCCCCCCCCCCCCAAAAAAAAACUCCCGCUUUUUUUUUUUUUUUUUAAAAAAAAGGGGGGGGGUCCCGCCUUUUUUUUUAAAAAAAACCCCCCCCGGGGGGGGGCCCGCCCUUUUUUUUUAAAAAAAAAAAAAAAAUUUUUUUUUCCGCCCCUUUUUUUUUUUUUUUUUAAAAAAAAAAAAAAAAACGCCCCUUUUUUUUUUGGGGGGGGCCCCCCCCCCCCCCCCCGCCCCUCUUUUUUUUUGGGGGGGGCCCCCCCCAAAAAAAAACCCCUCUUUUUUUUUUCCCCCCCCGGGGGGGGGUUUUUUUUCCCUCUCUUUUUUUUUGGGGGGGGCCCCCCCCCUUUUUUUUCCUCUCCGUUUUUUUUUGGGGGGGGUUUUUUUUUUUUUUUUUUCUCCGCUUUUUUUUUCCCCCCCCAAAAAAAAGGGGGGGGGCUCCGCCUUUUUUUUUGGGGGGGGAAAAAAAAUUUUUUUUUUCCGCCCUUUUUUUUUCCCCCCCCAAAAAAAACCCCCCCCCCCGCCCAUUUUUUUUUGGGGGGGGUUUUUUUUUUUUUUUUUCGCCCACUUUUUUUUUUUUUUUUUAAAAAAAACCCCCCCCCGCCCACUCUUUUUUUUGGGGGGGGCCCCCCCCCAAAAAAAACCCACUCCUUUUUUUUGGGGGGGGGGGGGGGGGUUUUUUUUCCACUCCCUUUUUUUUCCCCCCCCAAAAAAAAAAAAAAAAACACUCCCGUUUUUUUUCCCCCCCCCCCCCCCCCAAAAAAAAACUCCCGCUUUUUUUUCCCCCCCCCCCCCCCCCAAAAAAAACUCCCGCCUUUUUUUUUUUUUUUUUAAAAAAAAGGGGGGGGUCCCGCCCUUUUUUUUAAAAAAAAACCCCCCCCGGGGGGGGCCCGCCCCUUUUUUUUUAAAAAAAAAAAAAAAAUUUUUUUUUCGCCCCUUUUUUUUUUUUUUUUUUAAAAAAAAAAAAAAAAAGCCCCUCUUUUUUUUUGGGGGGGGCCCCCCCCCCCCCCCCCCCCCUCUUUUUUUUUUGGGGGGGGCCCCCCCCAAAAAAAAACCCUCUCUUUUUUUUUCCCCCCCCGGGGGGGGGCUUUUUUUUUUCUCCGCUUUUUUUUUGGGGGGGGCCCCCCCCUUUUUUUUUCUCCGCCUUUUUUUUUGGGGGGGGUUUUUUUUUUUUUUUUUUCCGCCCUUUUUUUUUCCCCCCCCAAAAAAAAGGGGGGGGGCCGCCCAUUUUUUUUUGGGGGGGGAAAAAAAAUUUUUUUUUCGCCCACUUUUUUUUUCCCCCCCCAAAAAAAAACCCCCCCCGCCCACUCUUUUUUUUGGGGGGGGUUUUUUUUUUUUUUUUUCCCACUCCUUUUUUUUUUUUUU
```

Because the beeps last for several 0.1 second chunks, the letters repeat when they shouldn't so we need to clean that up. We will round based on 0.85 seconds per beep to clean up the letters.<br>
Then we run 4 rounds of the decoding since we don't know if we started at the middle of the first word. For example, `UAGCUAGCUAGC`. We don't know if the correct string should be `UAGC UAGC UAGC` or if we caught it in the middle and it should be `UA GCUA GCUA GC`.<br>

```python
import re
import numpy as np
import matplotlib.pyplot as plt
from scipy.io import wavfile

# ==========================================
# 1. LOAD THE AUDIO
# ==========================================
# Replace with your actual audio file name
sample_rate, data = wavfile.read('SIG_INT_1774003543049.wav')

# If it's stereo, grab just the first channel
if len(data.shape) > 1: 
    data = data[:, 0]


print("Opening visual spectrogram window...")
print("\n==========================================")

# ==========================================
# 2. VISUALIZATION (SPECTROGRAM)
# ==========================================
plt.figure(figsize=(12, 6))
plt.specgram(data, Fs=sample_rate, NFFT=1024, noverlap=512, cmap='viridis')

plt.title('Waterfall Analysis (Spectrogram)')
plt.ylabel('Frequency [Hz]')
plt.xlabel('Time [sec]')
plt.colorbar(label='Intensity [dB]')

# Display the graph (This will pause the script until the window is closed)
plt.show()

print("Analyzing audio signal mathematically...\n")

# ==========================================
# 3. DIGITAL SIGNAL PROCESSING (DECODING)
# ==========================================
# Based on LOG_088: A=0, C=1, G=2, U=3
freq_map = {1000: 'A', 2000: 'U', 3000: 'G', 4000: 'C'}
targets = np.array([1000, 2000, 3000, 4000])

# Phase 1: FFT Slicing (0.1s chunks)
chunk_size = int(sample_rate * 0.1)  # 0.1 seconds per pulse
sequence = []

for i in range(0, len(data), chunk_size):
    chunk = data[i:i+chunk_size]
    if len(chunk) < chunk_size: 
        break
    
    # FFT to find the loudest frequency in this chunk
    fft_data = np.abs(np.fft.rfft(chunk))
    freqs = np.fft.rfftfreq(len(chunk), 1/sample_rate)
    
    # Find which target frequency is loudest
    peak_idx = np.argmax(fft_data)
    peak_freq = freqs[peak_idx]
    
    # Match to the closest target (1k, 2k, 3k, or 4k)
    closest_freq = targets[np.argmin(np.abs(targets - peak_freq))]
    sequence.append(freq_map[closest_freq])

full_string = "".join(sequence)

# Phase 2 & 3: Noise Filter & RLE Chunking
matches = re.finditer(r'(A{6,}|C{6,}|G{6,}|U{6,})', full_string)
aligned_sequence = ""

for m in matches:
    char = m.group(1)[0]
    length = len(m.group(1))
    base_count = round(length / 8.5)
    aligned_sequence += char * max(1, base_count)

# Phase 4: Decoding the 4 Offsets
mapping = {'A': '00', 'U': '01', 'G': '10', 'C': '11'}
print("--- Decoding Results ---")

for offset in range(4):
    shifted = aligned_sequence[offset:]
    result = ""
    
    for i in range(0, len(shifted) - 3, 4):
        codon = shifted[i:i+4]
        bits = "".join(mapping[b] for b in codon)
        val = int(bits, 2)
        
        # Filter for printable ASCII characters
        if 32 <= val <= 126:
            result += chr(val)
        else:
            result += "."
            
    print(f"Offset {offset}: {result}")
```

Running this gives us the password `|RNAPolymeraseSlip|`:<br>
<img width="459" height="226" alt="2026-05-03_19-51-53" src="https://github.com/user-attachments/assets/9b146f06-5df6-47f2-b420-ba4c3f84fc82" />

We can use this against the zip file we were given:<br>
<img width="566" height="364" alt="2026-05-03_20-03-09" src="https://github.com/user-attachments/assets/92c7c0d7-1771-467f-9553-9b76e6864d8f" />

To extract the flag!<br>
<img width="317" height="47" alt="2026-05-03_20-03-26" src="https://github.com/user-attachments/assets/aee76a6c-0f8a-4230-bb32-da21f88b0bff" />


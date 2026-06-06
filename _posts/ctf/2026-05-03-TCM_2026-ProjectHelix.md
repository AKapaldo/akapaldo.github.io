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

## TL;DR
Project Helix is a multi-stage forensic and cryptography challenge. The scenario involves investigating the disappearance of a lead geneticist, requiring the forensic analysis of a provided Windows triage image. The attack path requires parsing the NTFS Master File Table (MFT) to recover deleted file metadata, exploiting predictable web directories to recover deleted logs, tuning a hidden web-radio interface, and utilizing Digital Signal Processing (DSP) to decode a synthetic RNA frequency broadcast into an ASCII password.

### Attack Path Visualization
```mermaid
flowchart TD
    %% Stage 1
    subgraph S1 [Stage 1: Digital Forensics]
        direction LR
        A[Windows Triage Image] -->|Parse $MFT| B(Identify Deleted freq.txt)
        B -->|Extract Zone.Identifier| C[Discover Host URL]
    end

    %% Stage 2
    subgraph S2 [Stage 2: Web Enumeration]
        direction LR
        D[Fuzz 2-Byte URL Hex] -->|Size Differential| E(Discover Valid Directory)
        E -->|Review freq.txt| F[Find Hidden Radio Frequency]
    end

    %% Stage 3
    subgraph S3 [Stage 3: Signal Processing]
        direction LR
        G[Tune Web Radio to 3817.3] -->|Record Output| H[Generate Audio File]
        H -->|Spectrogram Analysis| I[Map Frequencies to RNA Bases]
    end

    %% Stage 4
    subgraph S4 [Stage 4: Cryptography]
        direction LR
        J[Convert Audio to RNA String] -->|Base-4 to Binary Decode| K(Extract Password)
        K -->|Unzip Archive| L((TCM_FLAG))
    end

    %% Cross-stage links
    S1 -->|Recover Download Origin| S2
    S2 -->|Identify Target Signal| S3
    S3 -->|Digital Signal Processing| S4

    %% Styling
    style S1 fill:transparent,stroke:#555,stroke-width:2px,stroke-dasharray: 5 5
    style S2 fill:transparent,stroke:#555,stroke-width:2px,stroke-dasharray: 5 5
    style S3 fill:transparent,stroke:#555,stroke-width:2px,stroke-dasharray: 5 5
    style S4 fill:transparent,stroke:#555,stroke-width:2px,stroke-dasharray: 5 5
```

## Solve
### Phase 1: Forensic Triage & The MFT

We are provided with `2026-02-25T224057_Triage.zip`, which extracts to a raw copy of a Windows `C:\` drive. Exploring the `C:\Users\drowens` directory, we find a shortcut to a file named `freq.txt` inside the `C:\users\drowens\AppData\Roaming\Microsoft\Windows\Recent` files folder.


I right-clicked to review the properties of the file:<br>
<img width="405" height="509" alt="2026-03-17_20-05-49" src="https://github.com/user-attachments/assets/80e84a80-2525-4c71-8162-31de591f7d86" />

The shortcut points to `C:\users\drowens\Downloads`, but this directory is missing from the triage image, indicating the file was deleted to hide the research. To recover data about this deleted file, we must analyze the file system's index.

Using [Eric Zimmermans](https://ericzimmerman.github.io/) MFT Explorer, we load the `C:\$MFT` file and search for `freq.txt`:<br>
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/6d8d6ab9-b542-4be2-89fe-8089b48ac8b8" />

In NTFS file systems, the `$MFT` acts as a massive database tracking every file, including metadata for deleted files that haven't been overwritten. By inspecting the `freq.txt` record, we can view its Alternate Data Streams (ADS), specifically the `Zone.Identifier`. This is known as the "Mark of the Web" (MotW), a security feature Windows uses to track the origin URL of downloaded files.

The Zone Identifier reveals the file was downloaded from: `https://ctf.tcmsecurity.com/3c7d7997..a7/freq.txt`.

### Phase 2: Web Fuzzing & Size Differentials

The extracted URL contains two corrupted bytes `(..)`. filling in any letters will give you a website since this is a strict Single Page Application (SPA) proxy so we can use a Size Differential Attack.

Assuming the missing bytes are standard hexadecimal characters, we can write a PowerShell script to iterate from `00` to `FF`. We check the Content.Length of a known bad request and compare it against our fuzzed requests to find the outlier.
<img width="1096" height="60" alt="image" src="https://github.com/user-attachments/assets/e0abe21f-3f99-428e-b663-015716914392" />

```powershell
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
The script identifies `c1a7` as the correct sequence:<br>
<img width="1092" height="191" alt="2026-05-03_17-59-39" src="https://github.com/user-attachments/assets/75f1d675-d904-480a-a829-20c5d08d8e65" />

{: .notice--info}

><strong>Alternative method:</strong><br>
> Instead of relying on a GUI tool, we can extract this data directly from the $MFT using Linux utilities.
> We can `grep` on the part that we know is good in the MFT, `3c7d7997` using `-a` to treat the file as text, `-b` to get the byte offset, and `-o` to only print the pattern.<br>
> MFT records are exactly 1024 bytes long, so they always start at multiples of 1024. The byte offset from above is in the middle of a record, so we use floor division to give us a whole number for `212669942 // 1024`.
> Then we multiply by 1024 to find the starting byte of the MFT record to inspect, giving us `212669440`.<br>
> Now we can use `dd` to find the data. `if=` for the input file, `bs=1` to set the block size to 1 byte, `skip=212669440` to start at the boundry we calculated, `count=100` to read the first 100 bytes after, and the `| xxd` to convert the binary to a hex dump we can read.<br>
><img width="666" height="385" alt="2026-03-17_21-38-23" src="https://github.com/user-attachments/assets/ef7e698e-f599-426e-8694-b551570e48fd" />

Navigating to the restored URL reveals the deleted `freq.txt` log, which contains a list of radio frequencies. <br>
<img width="1913" height="979" alt="2026-03-17_21-35-55" src="https://github.com/user-attachments/assets/dda7b1ed-e46e-4314-8109-d6500ff90ac5" />

Filtering out the standard entries using `grep -v "(X)" freq.txt` isolates a single anomaly: `3817.3`:<br>
<img width="1350" height="559" alt="2026-03-17_21-41-34" src="https://github.com/user-attachments/assets/01667d04-8aa5-4a9f-a59d-c2f541864447" />

### Phase 3: Web Radio & Spectrogram Analysis

Removing the `freq.txt` from the from the URL drops us onto a web-based radio interface with `volume`, `RF gain`, `zoom`, and `radio freq` dials.:<br>
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

Using Python and `matplotlib`, we plot the audio file into a Spectrogram to visualize the frequencies over time.:
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

A spectrogram (or waterfall graph) allows us to see the exact frequencies (in Hz) being broadcast. Zooming in on our graph reveals the beeps are strictly modulating between four distinct frequencies: 1000 Hz, 2000 Hz, 3000 Hz, and 4000 Hz:<br>
<img width="1202" height="674" alt="2026-05-03_18-56-49" src="https://github.com/user-attachments/assets/cb5c5e03-76e2-4724-8350-9d247354e352" />

Cross-referencing this with the clue found in `LOG_088` in the `freq.txt` file we can map the letters to frequencies:
```
LOG_088:
It speaks in strands. A four-fold tongue. I saw it on the waterfall today. tHEY are sending a message... quaternary? every pulse is part of a base... A...C...G...U... but what is the charcode? They called me CRAZY!

If I start counting from 0 it starts to make sense.
```

So we make A=0, C=1, G=2, and U=3 or A=1000, C=2000, G=3000, and U=4000.

### Phase 4: Signal Decoding & Base-4 Translation

With the audio mapped, we utilize Python 'scipy` to slice the audio into 0.1-second chunks, identify the peak frequency in each chunk, and generate a massive string of raw RNA bases (e.g., AAAAAAAACCCCCCCCCGCCC...).
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

The final hurdle is translating this synthetic RNA into an ASCII password. This requires understanding base-4 (quaternary) computing.
- There are 4 RNA bases, which can be represented by 2 bits of binary: A=00, U=01, G=10, C=11.
- A standard ASCII character is 1 byte (8 bits).
- Therefore, it takes exactly 4 RNA bases (4 bases × 2 bits) to equal 1 ASCII character.

Because we don't know if our recording started exactly at the beginning of a character byte, our script must test all 4 possible offsets to see which one perfectly aligns the binary into readable text.

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

The script successfully translates the signal on the correct offset, revealing the archive password: `|RNAPolymeraseSlip|`:<br>
<img width="459" height="226" alt="2026-05-03_19-51-53" src="https://github.com/user-attachments/assets/9b146f06-5df6-47f2-b420-ba4c3f84fc82" />

We use this password to decrypt the high-risk specimen archive and extract the flag.:<br>
<img width="566" height="364" alt="2026-05-03_20-03-09" src="https://github.com/user-attachments/assets/92c7c0d7-1771-467f-9553-9b76e6864d8f" />

To extract the flag!<br>
<img width="317" height="47" alt="2026-05-03_20-03-26" src="https://github.com/user-attachments/assets/aee76a6c-0f8a-4230-bb32-da21f88b0bff" />

## Flag
`TCM{V01D_S1GN4L_4U7H3N71C473D}`

## Vulnerability Mapping: Common Weakness Enumerations (CWE)
| CWE ID | Vulnerability Name | Application in Challenge |
|--------|--------------------|--------------------------|
| CWE-311 | Missing Encryption of Sensitive Data | The biological broadcast transmitted the password in plaintext over an open, unencrypted radio frequency, relying solely on obscurity and data encoding (Base-4) rather than actual cryptography. |
| CWE-534 | Information Exposure Through Debug Log Files | The development/research logs left on the active server leaked the critical frequency needed to intercept the broadcast. |


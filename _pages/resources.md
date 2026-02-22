---
layout: single
title: "Resources"
permalink: /resources/
author_profile: true
classes: wide
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: /assets/images/hero-banner.jpg
excerpt: "Tools, references, and cheat sheets I use for security research and CTF competitions"
---

<style>
.resources-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.2em;
  margin: 1.5em 0 2.5em;
}

.resource-card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px;
  padding: 1.2em 1.4em;
  transition: border-color 0.2s, transform 0.2s;
  display: flex;
  flex-direction: column;
  gap: 0.4em;
}

.resource-card:hover {
  border-color: #1f6feb;
  transform: translateY(-2px);
}

.resource-card a {
  text-decoration: none;
}

.resource-card__title {
  font-size: 1em;
  font-weight: 600;
  color: #58a6ff;
  display: flex;
  align-items: center;
  gap: 0.5em;
}

.resource-card__title i {
  color: #58a6ff;
  font-size: 0.9em;
  width: 16px;
  text-align: center;
}

.resource-card__desc {
  font-size: 0.85em;
  color: #8b949e;
  line-height: 1.5;
}

.resource-card__tag {
  display: inline-block;
  font-size: 0.72em;
  padding: 2px 8px;
  border-radius: 20px;
  border: 1px solid;
  margin-top: 0.3em;
  width: fit-content;
}

.tag-web     { color: #238636; border-color: rgba(35,134,54,0.4); background: rgba(35,134,54,0.08); }
.tag-forensics{ color: #1f6feb; border-color: rgba(31,111,235,0.4); background: rgba(31,111,235,0.08); }
.tag-malware { color: #da3633; border-color: rgba(218,54,51,0.4); background: rgba(218,54,51,0.08); }
.tag-network { color: #17a2b8; border-color: rgba(23,162,184,0.4); background: rgba(23,162,184,0.08); }
.tag-osint   { color: #e3b341; border-color: rgba(227,179,65,0.4); background: rgba(227,179,65,0.08); }
.tag-general { color: #8b949e; border-color: rgba(139,148,158,0.4); background: rgba(139,148,158,0.08); }
.tag-crypto  { color: #f6ff91; border-color: rgba(246,255,145,0.4); background: rgba(246,255,145,0.08); }
.tag-exploit { color: #bc8cff; border-color: rgba(188,140,255,0.4); background: rgba(188,140,255,0.08); }
.tag-stego   { color: #fd7e14; border-color: rgba(253,126,20,0.4); background: rgba(253,126,20,0.08); }

.section-intro {
  color: #8b949e;
  font-size: 0.95em;
  margin-bottom: 0.5em;
}
</style>

## 🛠️ Tools & Software

<p class="section-intro">Security tools I reach for regularly during CTF competitions and day-to-day security work.</p>

<div class="resources-grid">

  <div class="resource-card">
    <a href="https://portswigger.net/burp" target="_blank">
      <div class="resource-card__title"><i class="fas fa-spider"></i> Burp Suite</div>
    </a>
    <div class="resource-card__desc">The industry-standard web application security testing platform. Intercept, modify, and replay HTTP/S traffic with a powerful suite of tools for finding web vulnerabilities.</div>
    <span class="resource-card__tag tag-web">Web</span>
  </div>

  <div class="resource-card">
    <a href="https://www.wireshark.org" target="_blank">
      <div class="resource-card__title"><i class="fas fa-network-wired"></i> Wireshark</div>
    </a>
    <div class="resource-card__desc">The world's foremost network protocol analyzer. Capture and interactively browse traffic running on a computer network for deep inspection of network packets.</div>
    <span class="resource-card__tag tag-network">Network</span>
  </div>

  <div class="resource-card">
    <a href="https://gchq.github.io/CyberChef" target="_blank">
      <div class="resource-card__title"><i class="fas fa-utensils"></i> CyberChef</div>
    </a>
    <div class="resource-card__desc">A web-based tool for performing all manner of encoding, decoding, encryption, compression, and data analysis operations. Supports chaining multiple operations together.</div>
    <span class="resource-card__tag tag-general">General</span>
  </div>

  <div class="resource-card">
    <a href="https://nmap.org" target="_blank">
      <div class="resource-card__title"><i class="fas fa-search"></i> Nmap</div>
    </a>
    <div class="resource-card__desc">The essential network discovery and security auditing tool. Scan hosts for open ports, running services, OS detection, and more using a flexible scripting engine.</div>
    <span class="resource-card__tag tag-network">Network</span>
  </div>

  <div class="resource-card">
    <a href="https://www.metasploit.com" target="_blank">
      <div class="resource-card__title"><i class="fas fa-terminal"></i> Metasploit Framework</div>
    </a>
    <div class="resource-card__desc">The world's most widely used penetration testing framework. Provides a comprehensive library of exploits, payloads, and auxiliary modules for security testing.</div>
    <span class="resource-card__tag tag-exploit">Exploitation</span>
  </div>

  <div class="resource-card">
    <a href="https://www.splunk.com" target="_blank">
      <div class="resource-card__title"><i class="fas fa-chart-bar"></i> Splunk</div>
    </a>
    <div class="resource-card__desc">A powerful platform for searching, monitoring, and analyzing machine-generated data. Used for SIEM operations, log analysis, threat hunting, and security dashboards.</div>
    <span class="resource-card__tag tag-general">General</span>
  </div>

</div>

---

## 📋 Cheat Sheets & References

<p class="section-intro">References I keep bookmarked for CTF work and security research.</p>

<div class="resources-grid">

  <div class="resource-card">
    <a href="https://gtfobins.github.io" target="_blank">
      <div class="resource-card__title"><i class="fab fa-linux"></i> GTFOBins</div>
    </a>
    <div class="resource-card__desc">A curated list of Unix binaries that can be used to bypass local security restrictions. Essential reference for privilege escalation and living-off-the-land techniques on Linux.</div>
    <span class="resource-card__tag tag-exploit">Exploitation</span>
  </div>

  <div class="resource-card">
    <a href="https://github.com/swisskyrepo/PayloadsAllTheThings" target="_blank">
      <div class="resource-card__title"><i class="fas fa-code"></i> PayloadsAllTheThings</div>
    </a>
    <div class="resource-card__desc">A comprehensive list of useful payloads and bypass techniques for web application security testing. Covers SQLi, XSS, SSRF, XXE, command injection, and much more.</div>
    <span class="resource-card__tag tag-web">Web</span>
  </div>

  <div class="resource-card">
    <a href="https://lolbas-project.github.io" target="_blank">
      <div class="resource-card__title"><i class="fab fa-windows"></i> LOLBAS</div>
    </a>
    <div class="resource-card__desc">Living Off The Land Binaries, Scripts, and Libraries — the Windows equivalent of GTFOBins. Documents every Windows binary that can be abused for offensive security purposes.</div>
    <span class="resource-card__tag tag-exploit">Exploitation</span>
  </div>

  <div class="resource-card">
    <a href="https://www.revshells.com" target="_blank">
      <div class="resource-card__title"><i class="fas fa-plug"></i> RevShells</div>
    </a>
    <div class="resource-card__desc">An interactive reverse shell generator supporting dozens of languages and techniques. Quickly generate reverse shell one-liners for any scenario with configurable IP and port.</div>
    <span class="resource-card__tag tag-exploit">Exploitation</span>
  </div>

  <div class="resource-card">
    <a href="https://sherlock-project.github.io" target="_blank">
      <div class="resource-card__title"><i class="fas fa-user-secret"></i> Sherlock</div>
    </a>
    <div class="resource-card__desc">Hunt down social media accounts by username across hundreds of social networks. A go-to tool for OSINT investigations and username enumeration challenges.</div>
    <span class="resource-card__tag tag-osint">OSINT</span>
  </div>

  <div class="resource-card">
    <a href="https://github.com/trustedsec/social-engineer-toolkit" target="_blank">
      <div class="resource-card__title"><i class="fas fa-people-arrows"></i> Social Engineer Toolkit</div>
    </a>
    <div class="resource-card__desc">An open-source penetration testing framework designed for social engineering attacks. Supports phishing, credential harvesting, and a wide range of human-focused attack vectors.</div>
    <span class="resource-card__tag tag-exploit">Exploitation</span>
  </div>

  <div class="resource-card">
    <a href="https://github.com/danielmiessler/SecLists" target="_blank">
      <div class="resource-card__title"><i class="fas fa-list"></i> SecLists</div>
    </a>
    <div class="resource-card__desc">The security tester's companion — a collection of multiple types of lists used during security assessments. Includes usernames, passwords, URLs, fuzzing payloads, and much more.</div>
    <span class="resource-card__tag tag-general">General</span>
  </div>

  <div class="resource-card">
    <a href="https://aperisolve.com" target="_blank">
      <div class="resource-card__title"><i class="fas fa-image"></i> AperiSolve</div>
    </a>
    <div class="resource-card__desc">An online steganography analysis platform that runs multiple stego tools automatically on uploaded images. Quickly checks for hidden data using zsteg, steghide, binwalk, and more.</div>
    <span class="resource-card__tag tag-stego">Steganography</span>
  </div>

</div>

---

<p class="notice--primary">Have a tool or resource suggestion? Feel free to reach out via <a href="https://github.com/AKapaldo">GitHub</a> or <a href="https://www.linkedin.com/in/andrew-kapaldo/">LinkedIn</a>.</p>

---
layout: single
title: "Tools & Projects"
permalink: /projects/
author_profile: true
classes: wide
header:
  overlay_color: "#1e3a5f"
  overlay_filter: "0.8"
  overlay_image: /assets/images/hero-banner.jpg
excerpt: "Personal projects, tools, and scripts I've built for security work and IT automation"
---

<style>
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1.4em;
  margin: 1.5em 0 2.5em;
}

.project-card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px;
  padding: 1.4em;
  display: flex;
  flex-direction: column;
  gap: 0.6em;
  transition: border-color 0.2s, transform 0.2s;
}

.project-card:hover {
  border-color: #1f6feb;
  transform: translateY(-2px);
}

.project-card__header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 1em;
}

.project-card__title {
  font-size: 1.05em;
  font-weight: 700;
  color: #e6edf3;
}

.project-card__github {
  color: #8b949e;
  font-size: 1.1em;
  transition: color 0.2s;
  flex-shrink: 0;
}

.project-card__github:hover {
  color: #58a6ff;
}

.project-card__desc {
  font-size: 0.87em;
  color: #8b949e;
  line-height: 1.6;
  flex: 1;
}

.project-card__footer {
  display: flex;
  align-items: center;
  gap: 0.6em;
  flex-wrap: wrap;
  margin-top: 0.2em;
}

.lang-tag {
  display: inline-flex;
  align-items: center;
  gap: 0.35em;
  font-size: 0.75em;
  padding: 3px 10px;
  border-radius: 20px;
  border: 1px solid;
  font-weight: 500;
}

.lang-python     { color: #3572A5; border-color: rgba(53,114,165,0.5); background: rgba(53,114,165,0.1); }
.lang-powershell { color: #012456; border-color: rgba(1,36,86,0.5);    background: rgba(1,36,86,0.3); color: #5bc4f5; }
.lang-batch      { color: #c0c0c0; border-color: rgba(192,192,192,0.3); background: rgba(192,192,192,0.07); }

.cat-tag {
  display: inline-flex;
  align-items: center;
  gap: 0.35em;
  font-size: 0.75em;
  padding: 3px 10px;
  border-radius: 20px;
  border: 1px solid;
}

.cat-it    { color: #1f6feb; border-color: rgba(31,111,235,0.4); background: rgba(31,111,235,0.08); }
.cat-alexa { color: #00caff; border-color: rgba(0,202,255,0.4);  background: rgba(0,202,255,0.08); }

.section-intro {
  color: #8b949e;
  font-size: 0.95em;
  margin-bottom: 0.5em;
}
</style>

## 🖥️ IT & Automation Tools

<p class="section-intro">Scripts and utilities built to solve real IT problems — file management, system cleanup, and Windows administration.</p>

<div class="projects-grid">

  <div class="project-card">
    <div class="project-card__header">
      <div class="project-card__title">OfficeConvert</div>
      <a href="https://github.com/AKapaldo/OfficeConvert" target="_blank" class="project-card__github" title="View on GitHub">
        <i class="fab fa-github"></i>
      </a>
    </div>
    <div class="project-card__desc">
      A batch utility for converting legacy Microsoft Office files to modern formats. Automatically processes directories of old <code>.xls</code> and <code>.doc</code> files into <code>.xlsx</code> and <code>.docx</code>, helping organizations modernize aging document libraries without manual effort.
    </div>
    <div class="project-card__footer">
      <span class="lang-tag lang-batch"><i class="fas fa-terminal"></i> Batch</span>
      <span class="cat-tag cat-it"><i class="fas fa-briefcase"></i> IT Automation</span>
    </div>
  </div>

  <div class="project-card">
    <div class="project-card__header">
      <div class="project-card__title">ROT — Redundant, Obsolete & Trivial File Locator</div>
      <a href="https://github.com/AKapaldo/ROT" target="_blank" class="project-card__github" title="View on GitHub">
        <i class="fab fa-github"></i>
      </a>
    </div>
    <div class="project-card__desc">
      A PowerShell tool for identifying ROT files — Redundant, Obsolete, and Trivial data cluttering file systems. Scans directories and surfaces candidates for cleanup, helping organizations reduce storage waste and comply with records management policies.
    </div>
    <div class="project-card__footer">
      <span class="lang-tag lang-powershell"><i class="fas fa-terminal"></i> PowerShell</span>
      <span class="cat-tag cat-it"><i class="fas fa-briefcase"></i> IT Automation</span>
    </div>
  </div>

  <div class="project-card">
    <div class="project-card__header">
      <div class="project-card__title">CleanWSUS</div>
      <a href="https://github.com/AKapaldo/CleanWSUS" target="_blank" class="project-card__github" title="View on GitHub">
        <i class="fab fa-github"></i>
      </a>
    </div>
    <div class="project-card__desc">
      A PowerShell cleanup script that forces client PCs to re-register with WSUS and SCCM. Resolves common issues where endpoints fall out of sync with patch management infrastructure, ensuring machines remain compliant and visible to administrators.
    </div>
    <div class="project-card__footer">
      <span class="lang-tag lang-powershell"><i class="fas fa-terminal"></i> PowerShell</span>
      <span class="cat-tag cat-it"><i class="fas fa-briefcase"></i> IT Automation</span>
    </div>
  </div>

</div>

---

## 🔊 Alexa Skills

<p class="section-intro">Voice-enabled skills built for Amazon Alexa using Python and the Alexa Skills Kit.</p>

<div class="projects-grid">

  <div class="project-card">
    <div class="project-card__header">
      <div class="project-card__title">GarageHelper</div>
      <a href="https://github.com/AKapaldo/GarageHelper" target="_blank" class="project-card__github" title="View on GitHub">
        <i class="fab fa-github"></i>
      </a>
    </div>
    <div class="project-card__desc">
      An Alexa skill that provides quick vehicle maintenance information by voice. Ask for your car's oil weight, oil filter part number, or oil drain plug wrench size without leaving the garage or getting your hands dirty searching on your phone.
    </div>
    <div class="project-card__footer">
      <span class="lang-tag lang-python"><i class="fab fa-python"></i> Python</span>
      <span class="cat-tag cat-alexa"><i class="fas fa-microphone"></i> Alexa Skill</span>
    </div>
  </div>

  <div class="project-card">
    <div class="project-card__header">
      <div class="project-card__title">WhatsForDinner</div>
      <a href="https://github.com/AKapaldo/WhatsForDinner" target="_blank" class="project-card__github" title="View on GitHub">
        <i class="fab fa-github"></i>
      </a>
    </div>
    <div class="project-card__desc">
      An Alexa skill that takes the decision fatigue out of choosing where to eat. Uses the Google Places API to randomly select a nearby restaurant based on your location, so you can stop debating and start eating.
    </div>
    <div class="project-card__footer">
      <span class="lang-tag lang-python"><i class="fab fa-python"></i> Python</span>
      <span class="cat-tag cat-alexa"><i class="fas fa-microphone"></i> Alexa Skill</span>
    </div>
  </div>

</div>

---

<p class="notice--primary">All projects are open source and available on <a href="https://github.com/AKapaldo" target="_blank">GitHub</a>. Feel free to use, fork, or contribute!</p>

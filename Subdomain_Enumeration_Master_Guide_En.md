# 🌐 Subdomain Enumeration Master Guide
## Complete Cheat Sheet for Bug Bounty & Pentesting

> Version: 2026 Edition
>
> This guide is designed for Bug Bounty Hunters, Pentesters, and Red Teamers.
> Compatible with GitHub Markdown and Obsidian.

---

# 📖 Table of Contents

1. Introduction
2. Enumeration Methodology
3. Passive Enumeration
4. Active Enumeration
5. DNS Resolution
6. Live Host Discovery
7. Screenshots & Visual Recon
8. Archive Recon
9. Infrastructure Analysis
10. Professional Automation Pipelines
11. Common Mistakes
12. Real Bug Bounty Workflow

---

# 🎯 Introduction

Subdomain Enumeration is the process of discovering subdomains associated with a target domain.

Goals:

- Expand attack surface
- Discover forgotten assets
- Find staging environments
- Identify development systems
- Locate hidden applications
- Increase bug bounty coverage

Example:

target.com

Potential findings:

- api.target.com
- dev.target.com
- admin.target.com
- vpn.target.com
- staging.target.com

---

# 🗺️ Enumeration Methodology

A professional workflow:

1. Passive Enumeration
2. Active Enumeration
3. DNS Validation
4. Live Host Detection
5. Fingerprinting
6. Content Discovery
7. Vulnerability Assessment

Pipeline:

Passive → Active → Resolve → Live → Fingerprint → Hunt

---

# 🔍 Passive Enumeration

Passive enumeration does not directly interact with the target.

Advantages:

- Safe
- Fast
- Low noise

---

## 🔹 Subfinder

Recommended primary tool.

Installation:

```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

Basic:

```bash
subfinder -d target.com
```

Useful Flags:

```bash
-all
-recursive
-silent
-o output.txt
-timeout 30
```

Advanced:

```bash
subfinder -d target.com -all -recursive -silent
```

---

## 🔹 Assetfinder

Fast and lightweight.

```bash
assetfinder --subs-only target.com
```

Save Output:

```bash
assetfinder --subs-only target.com > assetfinder.txt
```

---

## 🔹 Findomain

```bash
findomain -t target.com
```

Output:

```bash
findomain -t target.com -u output.txt
```

---

## 🔹 Amass Passive

```bash
amass enum -passive -d target.com
```

Recommended:

```bash
amass enum -passive -src -d target.com
```

-src shows discovery sources.

---

# 🏆 Certificate Transparency Enumeration

---

## 🔹 crt.sh

Manual:

```bash
https://crt.sh/?q=%25.target.com
```

CLI:

```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json"
```

Extract Names:

```bash
curl -s "https://crt.sh/?q=%25.target.com&output=json" | jq -r '.[].name_value'
```

---

## 🔹 Chaos Dataset

ProjectDiscovery Dataset

```bash
chaos -d target.com
```

---

# ⚡ Active Enumeration

Active methods interact directly with DNS infrastructure.

---

## 🔹 Amass Active

```bash
amass enum -active -d target.com
```

Aggressive:

```bash
amass enum -active -brute -d target.com
```

---

## 🔹 Shuffledns

DNS Bruteforce

```bash
shuffledns -d target.com -w words.txt -r resolvers.txt
```

Using SecLists:

```bash
shuffledns -d target.com -w subdomains-top1million-5000.txt -r resolvers.txt
```

---

## 🔹 Gobuster DNS

```bash
gobuster dns -d target.com -w words.txt
```

Threads:

```bash
gobuster dns -d target.com -w words.txt -t 100
```

---

# 🌐 DNS Resolution

Raw subdomains are not enough.

Verify DNS records.

---

## 🔹 DNSX

Basic:

```bash
cat subs.txt | dnsx
```

Show Responses:

```bash
cat subs.txt | dnsx -resp
```

A Records:

```bash
cat subs.txt | dnsx -a -resp
```

---

# 🚀 Live Host Discovery

Many discovered subdomains are dead.

Find active assets.

---

## 🔹 HTTPX

Basic:

```bash
cat resolved.txt | httpx
```

Professional:

```bash
cat resolved.txt | httpx -title -tech-detect -status-code -follow-redirects -silent
```

Useful Features:

- Status Code Detection
- Technology Detection
- CDN Detection
- Title Extraction
- Redirect Tracking

---

# 🔬 Technology Fingerprinting

Using HTTPX:

```bash
httpx -tech-detect
```

Examples:

- Nginx
- Apache
- IIS
- Cloudflare
- WordPress
- React
- NextJS

---

# 📸 Screenshots

Visual inspection saves time.

---

## 🔹 Gowitness

```bash
gowitness file -f live.txt
```

Benefits:

- Screenshot Capture
- HTML Report
- Fast Review

---

## 🔹 Eyewitness

```bash
eyewitness --web -f live.txt
```

Provides:

- Screenshots
- Categorization
- Reports

---

# 📚 Archive Recon

Historical data often reveals hidden assets.

---

## 🔹 GAU

Get All URLs

```bash
gau target.com
```

Save:

```bash
gau target.com > gau.txt
```

---

## 🔹 Waymore

```bash
waymore -i target.com
```

Useful for:

- Historical URLs
- Archived Assets
- Parameters

---

# 🔄 Merging Results

Combine tools.

```bash
cat *.txt | sort -u > all_subdomains.txt
```

Count:

```bash
wc -l all_subdomains.txt
```

---

# 🏗️ Infrastructure Analysis

Resolve IPs:

```bash
cat subs.txt | dnsx -resp
```

Extract IPs:

```bash
cat subs.txt | dnsx -resp-only
```

Check ASN:

```bash
amass intel -asn
```

Goals:

- Discover cloud assets
- Identify third-party providers
- Find forgotten servers

---

# 🚀 Professional Workflow

Step 1:

```bash
subfinder -d target.com -all -silent
```

Step 2:

```bash
assetfinder --subs-only target.com
```

Step 3:

```bash
amass enum -passive -d target.com
```

Step 4:

```bash
sort -u
```

Step 5:

```bash
dnsx
```

Step 6:

```bash
httpx
```

Step 7:

```bash
gowitness
```

---

# 🏆 Full Automation Pipeline

```bash
subfinder -d target.com -all -recursive -silent > subs.txt

assetfinder --subs-only target.com >> subs.txt

amass enum -passive -d target.com >> subs.txt

sort -u subs.txt > unique.txt

cat unique.txt | dnsx -silent > resolved.txt

cat resolved.txt | httpx -title -tech-detect -status-code -silent > live.txt

gowitness file -f live.txt
```

---

# ⚠️ Common Mistakes

1. Using only one tool
2. Skipping DNS validation
3. Ignoring archived assets
4. Not taking screenshots
5. Missing recursive enumeration
6. Ignoring ASN infrastructure
7. Not deduplicating results

---

# 🎯 Real Bug Bounty Workflow

Recon Phase:

```bash
subfinder
assetfinder
amass
chaos
```

Validation:

```bash
dnsx
```

Live Hosts:

```bash
httpx
```

Visual Review:

```bash
gowitness
```

Content Discovery:

```bash
katana
ffuf
```

Vulnerability Hunting:

```bash
nuclei
manual testing
```

---

# 📚 Recommended Tools

| Tool | Purpose |
|--------|---------|
| Subfinder | Passive Enumeration |
| Assetfinder | Fast Discovery |
| Amass | Deep Enumeration |
| Chaos | Dataset Enumeration |
| DNSX | DNS Resolution |
| HTTPX | Live Detection |
| Shuffledns | Bruteforce |
| Gau | Archive URLs |
| Waymore | Historical Data |
| Gowitness | Screenshots |
| Katana | Crawling |
| Nuclei | Vulnerability Scanning |

---

# 🏁 Final Notes

Always combine multiple sources.

Never trust a single tool.

Enumeration quality directly impacts bug bounty success.

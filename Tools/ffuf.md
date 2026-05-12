# FFUF Cheat Sheet

## What is FFUF?

FFUF (Fuzz Faster U Fool) is a fast web fuzzing tool commonly used for:
- Directory/file discovery
- Subdomain discovery
- Virtual host fuzzing
- Parameter fuzzing
- API endpoint discovery

---

# Basic Syntax

```bash
ffuf -u http://target/FUZZ -w wordlist.txt
```

`FUZZ` = placeholder where ffuf inserts words from the wordlist.

---

# Common Wordlists

Common locations on Linux/Kali:

```bash
/usr/share/wordlists/dirbuster/
/usr/share/seclists/
```

Popular lists:

```bash
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

---

# Basic Directory Discovery

```bash
ffuf -u http://target/FUZZ -w common.txt
```

Example:

```bash
ffuf -u http://10.10.10.10/FUZZ -w common.txt
```

---

# Add File Extensions

```bash
ffuf -u http://target/FUZZ -w common.txt -e .php,.txt,.html
```

Useful for:
- PHP sites
- Config files
- Backup files

---

# Filter Status Codes

Show only successful responses:

```bash
ffuf -u http://target/FUZZ -w common.txt -mc 200
```

Multiple status codes:

```bash
ffuf -u http://target/FUZZ -w common.txt -mc 200,204,301,302,403
```

---

# Hide Certain Status Codes

Hide 404 responses:

```bash
ffuf -u http://target/FUZZ -w common.txt -fc 404
```

---

# Filter by Response Size

Hide responses with a specific size:

```bash
ffuf -u http://target/FUZZ -w common.txt -fs 0
```

Useful when:
- Servers return fake 200 responses
- Error pages always have same size

---

# Recursion

Automatically scan discovered directories:

```bash
ffuf -u http://target/FUZZ -w common.txt -recursion
```

Limit recursion depth:

```bash
ffuf -u http://target/FUZZ -w common.txt -recursion -recursion-depth 2
```

---

# Save Output

JSON output:

```bash
ffuf -u http://target/FUZZ -w common.txt -o results.json -of json
```

CSV output:

```bash
ffuf -u http://target/FUZZ -w common.txt -o results.csv -of csv
```

---

# Set Threads

Increase speed:

```bash
ffuf -u http://target/FUZZ -w common.txt -t 100
```

Default is usually 40.

Higher threads:
- Faster
- More noisy
- May trigger rate limits

---

# Add Headers

Add custom headers:

```bash
ffuf -u http://target/FUZZ -w common.txt -H "Authorization: Bearer TOKEN"
```

Add cookies:

```bash
ffuf -u http://target/FUZZ -w common.txt -b "PHPSESSID=abc123"
```

---

# Virtual Host (VHOST) Fuzzing

```bash
ffuf -u http://target -H "Host: FUZZ.target.com" -w subdomains.txt
```

Useful for finding hidden virtual hosts.

---

# Subdomain Discovery

```bash
ffuf -u http://FUZZ.target.com -w subdomains.txt
```

---

# Parameter Fuzzing

GET parameters:

```bash
ffuf -u "http://target/page.php?FUZZ=test" -w params.txt
```

POST parameters:

```bash
ffuf -u http://target/login \
-X POST \
-d "FUZZ=test" \
-H "Content-Type: application/x-www-form-urlencoded" \
-w params.txt
```

---

# Find Backup Files

```bash
ffuf -u http://target/FUZZ -w common.txt -e .bak,.old,.zip,.tar.gz
```

Common backup extensions:
- .bak
- .old
- .zip
- .tar.gz
- .swp

---

# Rate Limiting

Limit requests per second:

```bash
ffuf -u http://target/FUZZ -w common.txt -rate 50
```

Useful to avoid:
- Detection
- Blocking
- Crashing test environments

---

# Replay Proxy (Burp Suite)

Send matches to Burp:

```bash
ffuf -u http://target/FUZZ -w common.txt -replay-proxy http://127.0.0.1:8080
```

Great for:
- Manual testing
- Inspecting responses
- Modifying requests

---

# Auto Calibration

Helps filter false positives:

```bash
ffuf -u http://target/FUZZ -w common.txt -ac
```

Very useful on noisy targets.

---

# Match by Word Count

```bash
ffuf -u http://target/FUZZ -w common.txt -mw 20
```

---

# Match by Line Count

```bash
ffuf -u http://target/FUZZ -w common.txt -ml 10
```

---

# Match by Response Size

```bash
ffuf -u http://target/FUZZ -w common.txt -ms 512
```

---

# Verbose Output

```bash
ffuf -u http://target/FUZZ -w common.txt -v
```

---

# Silent Mode

```bash
ffuf -u http://target/FUZZ -w common.txt -s
```

Useful for scripting.

---

# Example Real-World Commands

## Quick Web Scan

```bash
ffuf -u http://target/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-fc 404 \
-ac
```

---

## PHP Website Scan

```bash
ffuf -u http://target/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-e .php,.txt,.html,.bak \
-mc 200,301,302,403
```

---

## VHOST Discovery

```bash
ffuf -u http://target \
-H "Host: FUZZ.target.com" \
-w subdomains.txt \
-fs 0
```

---

## API Fuzzing

```bash
ffuf -u http://api.target/FUZZ \
-w api.txt \
-H "Authorization: Bearer TOKEN" \
-mc 200,401,403
```

---

# Useful Response Codes

| Code | Meaning |
|------|----------|
| 200 | OK |
| 204 | No Content |
| 301 | Redirect |
| 302 | Temporary Redirect |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

# Common Filters

| Option | Meaning |
|--------|---------|
| -mc | Match status code |
| -fc | Filter status code |
| -fs | Filter size |
| -fw | Filter words |
| -fl | Filter lines |
| -ms | Match size |
| -mw | Match words |
| -ml | Match lines |

---

# Tips

- Use `-ac` often to reduce false positives
- Start with smaller wordlists before large ones
- Filter noisy responses with `-fs`
- Use Burp Suite replay for manual analysis
- Recursive scans can become very noisy
- High thread counts may trigger WAFs/rate limits
- Always ensure you have authorization to test targets

---

# Install FFUF

## Kali Linux

```bash
sudo apt install ffuf
```

## Go Install

```bash
go install github.com/ffuf/ffuf/v2@latest
```

---

# Help Menu

```bash
ffuf -h
```

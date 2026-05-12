# DIRB Cheat Sheet

## What is DIRB?

DIRB is a web content scanner used to discover:
- Hidden directories
- Web files
- Backup files
- Admin panels
- Sensitive endpoints

DIRB works by brute forcing paths using wordlists.

---

# Basic Syntax

```bash
dirb <url> [wordlist]
```

Example:

```bash
dirb http://10.10.10.10
```

Using a custom wordlist:

```bash
dirb http://10.10.10.10 common.txt
```

---

# Default Wordlists

Common DIRB wordlists:

```bash
/usr/share/dirb/wordlists/common.txt
/usr/share/dirb/wordlists/big.txt
/usr/share/dirb/wordlists/small.txt
```

---

# Basic Directory Scan

```bash
dirb http://target
```

---

# Use Custom Wordlist

```bash
dirb http://target /usr/share/seclists/Discovery/Web-Content/common.txt
```

---

# Scan with File Extensions

```bash
dirb http://target -X .php,.txt,.html
```

Useful for:
- PHP apps
- Config files
- Backup files

---

# Ignore Status Codes

Ignore 404 errors:

```bash
dirb http://target -N 404
```

Ignore multiple status codes:

```bash
dirb http://target -N 404,403
```

---

# Silent Mode

Less verbose output:

```bash
dirb http://target -S
```

---

# Save Output to File

```bash
dirb http://target -o results.txt
```

---

# Use Cookies

```bash
dirb http://target -c "PHPSESSID=abc123"
```

---

# Add Custom Headers

```bash
dirb http://target -H "Authorization: Bearer TOKEN"
```

---

# Use Proxy (Burp Suite)

```bash
dirb http://target -p http://127.0.0.1:8080
```

Useful for:
- Intercepting requests
- Manual testing
- Modifying traffic

---

# HTTP Authentication

Basic authentication:

```bash
dirb http://target -u admin:password
```

---

# Change User-Agent

```bash
dirb http://target -a "Mozilla/5.0"
```

---

# Recursive Scanning

Enable recursive scanning:

```bash
dirb http://target -r
```

Disable recursion:

```bash
dirb http://target -R
```

---

# Scan HTTPS Sites

```bash
dirb https://target
```

Ignore certificate warnings:

```bash
dirb https://target -k
```

---

# Search for Backup Files

```bash
dirb http://target -X .bak,.old,.zip,.tar.gz
```

Common backup extensions:
- .bak
- .old
- .zip
- .tar.gz
- .swp

---

# Scan Specific File Types

Only PHP files:

```bash
dirb http://target -X .php
```

Multiple extensions:

```bash
dirb http://target -X .php,.asp,.aspx
```

---

# Delay Requests

Add delay between requests:

```bash
dirb http://target -z 100
```

Useful for:
- Avoiding rate limits
- Reducing noise
- Slower stealthier scans

---

# Example Real-World Commands

## Quick Scan

```bash
dirb http://target
```

---

## PHP Website Scan

```bash
dirb http://target \
/usr/share/seclists/Discovery/Web-Content/common.txt \
-X .php,.txt,.html,.bak
```

---

## Scan Through Burp Suite

```bash
dirb http://target \
-p http://127.0.0.1:8080
```

---

## Authenticated Scan

```bash
dirb http://target \
-u admin:password \
-c "PHPSESSID=abc123"
```

---

# Common Response Codes

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

# Useful Options

| Option | Meaning |
|--------|---------|
| -X | File extensions |
| -N | Ignore status codes |
| -o | Output file |
| -p | Proxy |
| -H | Custom header |
| -c | Cookie |
| -u | HTTP auth |
| -a | User-Agent |
| -r | Recursive |
| -R | Disable recursion |
| -k | Ignore SSL errors |
| -z | Delay requests |
| -S | Silent mode |

---

# Tips

- Start with smaller wordlists first
- Use Burp Suite for manual analysis
- Scan backup extensions often
- Recursive scans can become noisy
- Use delays to avoid detection
- Custom headers help bypass protections
- Always ensure you have authorization to test targets

---

# Install DIRB

## Kali Linux

```bash
sudo apt install dirb
```

---

# Help Menu

```bash
dirb
```

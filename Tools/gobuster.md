# GOBUSTER Cheat Sheet

---

## Directory / File Brute Force (dir mode)

### Basic directory scan
```bash
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt
```

### Scan with extensions, show length, follow redirects, 50 threads, save output
```bash
gobuster dir -u https://example.com \
  -w /usr/share/wordlists/dirb/common.txt \
  -x php,html,js \
  -l -r -t 50 \
  -o gobuster-dir.txt
```

### Filter by status codes and set timeout
```bash
gobuster dir -u https://example.com \
  -w /path/words.txt \
  -s 200,301,302 \
  --timeout 10s \
  -t 30
```

### Add custom header and cookie (authenticated pages)
```bash
gobuster dir -u https://example.com \
  -w /path/words.txt \
  -H "Authorization: Bearer TOKEN" \
  -c "SESSION=abc123" \
  -t 20
```

### Recursive scan (nested directories)
```bash
gobuster dir -u https://example.com \
  -w /path/words.txt \
  --recursive \
  -t 25 \
  -o gobuster-dir-recursive.txt
```

---

## DNS Mode (Subdomain Discovery)

### Basic DNS bruteforce
```bash
gobuster dns -do example.com \
  -w /usr/share/wordlists/subdomains-top1million.txt \
  -t 50 \
  -o gobuster-dns.txt
```

### Custom DNS resolver
```bash
gobuster dns -do example.com \
  -w subdomains.txt \
  -r 8.8.8.8:53 \
  -t 80
```

---

## Virtual Host (vhost) Mode

### Append domain (vhost discovery)
```bash
gobuster vhost -u https://192.0.2.10 \
  --append-domain \
  -w /path/vhosts.txt \
  -t 40 \
  -o gobuster-vhost.txt
```

### Custom headers + skip TLS verification
```bash
gobuster vhost -u https://192.0.2.10 \
  --append-domain \
  -w vhosts.txt \
  -H "User-Agent: gobuster" \
  -k \
  -t 30
```

---

## Cloud / TFTP Modes

### S3 bucket bruteforce
```bash
gobuster s3 -w /path/buckets.txt -t 40 -o gobuster-s3.txt
```

### GCS bucket bruteforce
```bash
gobuster gcs -w /path/buckets.txt -t 40 -o gobuster-gcs.txt
```

### TFTP file discovery
```bash
gobuster tftp -s 10.0.0.5 -w /path/words.txt -t 20 -o gobuster-tftp.txt
```

---

## Fuzz Mode

### Fuzz a GET parameter
```bash
gobuster fuzz -u "https://example.com/search?query=FUZZ" \
  -w /path/words.txt \
  -t 40 \
  -o gobuster-fuzz.txt
```

### Fuzz a header value
```bash
gobuster fuzz -u https://example.com \
  -H "X-Api-Key: FUZZ" \
  -w /path/words.txt \
  -t 30
```

### Fuzz POST body
```bash
gobuster fuzz -u https://example.com/login \
  -d "username=admin&password=FUZZ" \
  -w /path/passwords.txt \
  -t 20
```

---

## Helpful Flags (Quick Reference)

```
-u <url>              Target URL (dir, vhost, fuzz)
-w <wordlist>         Wordlist path
-t <threads>          Concurrent threads (higher = faster)
-x <ext1,ext2>        Extensions (dir mode)
-o <file>             Save output
-s <codes>            Status codes to show (200,301,302)
-l                    Show response length
-r, --follow-redirect Follow redirects
-H, --headers         Custom headers (repeatable)
-c, --cookies         Send cookies
-k, --no-tls-validation Skip TLS verification
--timeout <dur>       Request timeout (e.g., 10s)
--append-domain       Append domain (vhost mode)
-do <domain>          DNS target (dns mode)
--help                Show help (e.g., gobuster help dir)
```

---

## Quick One-Liners

### Common directory scan
```bash
gobuster dir -u https://example.com \
  -w /usr/share/wordlists/dirb/common.txt \
  -t 40 \
  -o gobuster-dir.txt
```

### Dir scan with extensions + only 200/301/302
```bash
gobuster dir -u https://example.com \
  -w /path/words.txt \
  -x php,html,js \
  -s 200,301,302 \
  -t 50 \
  -o gobuster-dir-ext.txt
```

### Subdomain scan
```bash
gobuster dns -do example.com \
  -w /usr/share/wordlists/subdomains-top1million.txt \
  -t 100 \
  -o gobuster-dns.txt
```

### Vhost scan
```bash
gobuster vhost -u https://192.0.2.10 \
  --append-domain \
  -w /path/vhosts.txt \
  -t 60 \
  -o gobuster-vhost.txt
```

### S3 bucket check
```bash
gobuster s3 -w /path/buckets.txt -t 50 -o gobuster-s3.txt
```

---

## Tips & Safety

- Start with low thread counts (10–50) and increase carefully.
- Use targeted wordlists before huge lists.
- Use headers/cookies for authenticated testing.
- Always get written permission before testing. Unauthorized scanning is illegal.

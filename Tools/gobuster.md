# GOBUSTER 

###############
# Directory / file brute force (dir mode)
###############
# basic directory scan
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt

# scan with common extensions (comma-separated), show response length, follow redirects, 50 threads, save output
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt -x php,html,js -l -r -t 50 -o gobuster-dir.txt

# filter by status codes (show only 200, 301, 302) and set timeout
gobuster dir -u https://example.com -w /path/words.txt -s 200,301,302 --timeout 10s -t 30

# add custom header and cookie (useful for authenticated pages)
gobuster dir -u https://example.com -w /path/words.txt -H "Authorization: Bearer TOKEN" -c "SESSION=abc123" -t 20

# recursive scan (discover nested directories)
gobuster dir -u https://example.com -w /path/words.txt --recursive -t 25 -o gobuster-dir-recursive.txt

###############
# DNS (subdomain discovery) mode
###############
# basic DNS bruteforce (example uses README-style flag)
gobuster dns -do example.com -w /usr/share/wordlists/subdomains-top1million.txt -t 50 -o gobuster-dns.txt

# use a custom DNS resolver (e.g., 8.8.8.8)
gobuster dns -do example.com -w subdomains.txt -r 8.8.8.8:53 -t 80

###############
# Virtual host (vhost) mode
###############
# test Host header names against a webserver (append domain, useful for vhosts)
gobuster vhost -u https://192.0.2.10 --append-domain -w /path/vhosts.txt -t 40 -o gobuster-vhost.txt

# include custom headers, skip TLS verification
gobuster vhost -u https://192.0.2.10 --append-domain -w vhosts.txt -H "User-Agent: gobuster" -k -t 30

###############
# S3 / GCS / TFTP modes (cloud/tftp discovery)
###############
# S3 bucket name bruteforce (simple)
gobuster s3 -w /path/buckets.txt -t 40 -o gobuster-s3.txt

# GCS bucket bruteforce
gobuster gcs -w /path/buckets.txt -t 40 -o gobuster-gcs.txt

# TFTP file discovery (target IP example)
gobuster tftp -s 10.0.0.5 -w /path/words.txt -t 20 -o gobuster-tftp.txt

###############
# Fuzz mode (use FUZZ placeholder in URL, headers, or POST data)
###############
# fuzz a GET parameter
gobuster fuzz -u "https://example.com/search?query=FUZZ" -w /path/words.txt -t 40 -o gobuster-fuzz.txt

# fuzz a header value
gobuster fuzz -u https://example.com -H "X-Api-Key: FUZZ" -w /path/words.txt -t 30

# fuzz POST body
gobuster fuzz -u https://example.com/login -d "username=admin&password=FUZZ" -w /path/passwords.txt -t 20

###############
# Helpful defaults & flags (quick reference)
###############
# -u <url>           target URL (dir, vhost, fuzz)
# -w <wordlist>      wordlist path
# -t <threads>       number of concurrent threads (higher = faster)
# -x <ext1,ext2>     comma-separated extensions (dir mode)
# -o <file>          save output to file
# -s <codes>         comma-separated status codes to show (e.g., 200,301,302)
# -l                 show response length/size
# -r, --follow-redirect follow redirects
# -H, --headers      specify HTTP headers (can repeat -H "Header: value")
# -c, --cookies      send cookies (e.g., -c "SESSION=abc")
# -k, --no-tls-validation skip TLS cert verification
# --timeout <dur>    request timeout (e.g., 10s)
# --append-domain    append target domain to words (vhost mode)
# -do <domain>       DNS target (dns mode) — example: gobuster dns -do example.com -w words.txt
# --help             show help for a mode: gobuster help dir

###############
# Quick one-line examples (copy/paste)
###############
# common directory scan
gobuster dir -u https://example.com -w /usr/share/wordlists/dirb/common.txt -t 40 -o gobuster-dir.txt

# dir scan with extensions & only show 200/301/302
gobuster dir -u https://example.com -w /path/words.txt -x php,html,js -s 200,301,302 -t 50 -o gobuster-dir-ext.txt

# subdomain scan
gobuster dns -do example.com -w /usr/share/wordlists/subdomains-top1million.txt -t 100 -o gobuster-dns.txt

# vhost scan (append domain)
gobuster vhost -u https://192.0.2.10 --append-domain -w /path/vhosts.txt -t 60 -o gobuster-vhost.txt

# S3 bucket check
gobuster s3 -w /path/buckets.txt -t 50 -o gobuster-s3.txt

###############
# Tips & safety
###############
# - Use reasonable thread counts for remote servers to avoid DoS / detection. Start low (10-50) and increase carefully.
# - Prefer targeted wordlists before huge lists to reduce noise/time.
# - Use custom headers/cookies when testing authenticated areas (but avoid sending secrets to third parties).
# - Always get written permission before testing; unauthorized scanning is illegal.

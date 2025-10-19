# Nmap — cheat sheet

Below are common target specs, scan types, discovery options, and port selection examples — each command shown as an indented code block so you can paste it straight into a terminal.

---

Service/port scans and version detection

nmap -sV -p 80,443 --script http-title <IP> can show site/page titles, server versions, and sometimes virtual host hints.

## Basic options
-sT	TCP connect scan – complete three-way handshake  
-sS	TCP SYN – only first step of the three-way handshake  
-sU	UDP scan  
-F	Fast mode – scans the 100 most common ports  
-p[range]	Specifies a range of port numbers – -p- scans all the ports  


## Target specification
Scan a single IP  
    nmap 192.168.1.1

Scan specific IPs  
    nmap 192.168.1.1 192.168.2.1

Scan a range  
    nmap 192.168.1.1-254

Scan a domain  
    nmap scanme.nmap.org

Scan using CIDR notation (whole subnet)  
    nmap 192.168.1.0/24

Scan targets from a file (`targets.txt`)  
    nmap -iL targets.txt

Scan 100 random hosts  
    nmap -iR 100

Exclude listed hosts  
    nmap --exclude 192.168.1.1

---

## Scan types / transport
TCP SYN scan (default when run as root)  
    nmap -sS 192.168.1.1

TCP connect scan (default without root)  
    nmap -sT 192.168.1.1

UDP scan  
    nmap -sU 192.168.1.1

TCP ACK scan  
    nmap -sA 192.168.1.1

TCP Window scan  
    nmap -sW 192.168.1.1

TCP Maimon scan  
    nmap -sM 192.168.1.1

List targets only (no scanning)  
    nmap -sL 192.168.1.1-3

Disable port scanning (host discovery only)  
    nmap -sn 192.168.1.0/24

Disable host discovery (treat all hosts as online)  
    nmap -Pn 192.168.1.1-5

---

## Host discovery probes
TCP SYN discovery on specified ports (port 80 included by default)  
    nmap -PS22-25,80 192.168.1.1-5

TCP ACK discovery on specified ports (port 80 default)  
    nmap -PA22-25,80 192.168.1.1-5

UDP discovery on specified port(s) (UDP 40125 default)  
    nmap -PU53 192.168.1.1-5

ARP discovery (good for local networks)  
    nmap -PR 192.168.1.1/24

Never do DNS resolution  
    nmap -n 192.168.1.1

---

## Port selection
Scan a single port  
    nmap -p 21 192.168.1.1

Port range  
    nmap -p 21-100 192.168.1.1

Scan multiple TCP/UDP ports (example)  
    nmap -p U:53,T:21-25,80 192.168.1.1

Scan all ports (1–65535)  
    nmap -p- 192.168.1.1

Fast scan (top 100 ports)  
    nmap -F 192.168.1.1

Top X ports (example: top 2000)  
    nmap --top-ports 2000 192.168.1.1

Start at a specific port range edge (examples)  
    nmap -p-65535 192.168.1.1
    nmap -p0- 192.168.1.1

Scan by service name  
    nmap -p http,https 192.168.1.1

---

## Version, OS detection, scripts, traceroute
Service/version detection  
    nmap -sV 192.168.1.1

Version intensity (0–9; higher = more thorough/slower)  
    nmap -sV --version-intensity 8 192.168.1.1

Version light (faster, less accurate)  
    nmap -sV --version-light 192.168.1.1

Version all (intensity 9)  
    nmap -sV --version-all 192.168.1.1

Enable OS detection, version detection, script scanning, and traceroute  
    nmap -A 192.168.1.1

Remote OS detection (TCP/IP fingerprinting)  
    nmap -O 192.168.1.1

Limit OS detection attempts (skip if not enough info)  
    nmap -O --osscan-limit 192.168.1.1

Make OS detection guess more aggressively  
    nmap -O --osscan-guess 192.168.1.1

Set max OS detection tries  
    nmap -O --max-os-tries 1 192.168.1.1

---

## Timing / performance (T0–T5)
Paranoid (0) — IDS evasion  
    nmap -T0 192.168.1.1

Sneaky (1) — IDS evasion  
    nmap -T1 192.168.1.1

Polite (2) — slows scan to use less bandwidth/resources  
    nmap -T2 192.168.1.1

Normal (3) — default speed  
    nmap -T3 192.168.1.1

Aggressive (4) — faster scans on reliable networks  
    nmap -T4 192.168.1.1

Insane (5) — fastest; assumes extremely fast network  
    nmap -T5 192.168.1.1

---

## Examples / combos
Aggressive scan + OS + services + scripts + traceroute  
    nmap -A -T4 192.168.1.1

UDP scan of DNS + host discovery only  
    nmap -sU -PU53 192.168.1.1

Scan a subnet, show hosts only (no port scan)  
    nmap -sn 192.168.1.0/24

Scan a list of targets from file and exclude one IP  
    nmap -iL targets.txt --exclude 192.168.1.1

Version detection on top 200 ports only  
    nmap --top-ports 200 -sV 192.168.1.1

Scan all TCP ports, be polite (slower)  
    nmap -p- -T2 192.168.1.1

---

## Notes / tips
- Use `sudo` for scans that require raw socket access (e.g., `-sS`, `-O`).  
- ARP discovery (`-PR`) is fastest on local Ethernet segments.  
- `-Pn` is useful if ICMP is blocked or hosts deliberately drop pings.  
- Be mindful of legal/ethical rules — only scan networks you own or have permission to test.

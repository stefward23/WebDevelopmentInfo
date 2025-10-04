# Telnet — quick, copy-friendly Markdown cheat sheet

Below is a single, copyable Markdown block with common Telnet commands, connection examples, session controls, troubleshooting tips, and useful options. Paste the whole block where you want.

---

## Basic connection
Connect to a host on the default telnet port (23):  
    telnet host.example.com

Connect to a host on a specific port (example: 2323):  
    telnet host.example.com 2323

Connect by IP address:  
    telnet 192.168.1.100

Connect and immediately run a command (POSIX-only example using here-doc to send commands after connect)  
    telnet host.example.com 23 <<'EOF'
    your-command
    EOF

Use netcat as a lightweight telnet-like alternative (if telnet is unavailable):  
    nc host.example.com 23

---

## Web server/page
example:
GET / HTTP/1.1
Host: telnet.thm

Reverse DNS (PTR)

dig -x <IP> +short

nslookup <IP>

If there’s a PTR record you’ll get a hostname. Not guaranteed — many IPs don’t have useful PTRs.

---

## Common interactive commands (local telnet client commands)
When in a telnet session, enter the telnet command character (often `Ctrl+]`) to get the telnet prompt, then use these commands.

Open a connection (from telnet prompt)  
    open hostname [port]

Close the current connection  
    close

Quit telnet client  
    quit

Show status of current session(s)  
    status

Send an interrupt or break to remote (from telnet prompt):  
    send interrupt
    send brk

Switch between character and line mode (when supported)  
    mode character
    mode line

Turn local echo on/off (useful if remote doesn't echo)  
    toggle echo
    (or use `set` below to control options)

Display help for telnet prompt commands  
    help

---

## Client options (common command-line flags)
Suppress automatic DNS lookups (varies by implementation — check local manpage):  
    telnet -n host.example.com

Set telnet to use an alternate port (if client supports `-p`):  
    telnet -p 2323 host.example.com

Use a specific interface/source IP when connecting (BSD/OpenBSD telnet example — may vary):  
    telnet -b 192.168.1.50 host.example.com 23

Note: telnet client flags differ between OSes. Check `man telnet` or `telnet --help` on your system.

---

## Login & authentication
Standard login prompt (username/password) will appear when you connect to an interactive service. Typical flow:  
    (1) telnet host.example.com
    (2) Username: youruser
    (3) Password: ********

Telnet is plaintext — credentials and data are unencrypted. Prefer SSH for remote shell logins whenever possible.

---

## Escape sequences & keyboard control
Enter the telnet command prompt (common default):  
    Ctrl + ]

From telnet prompt you can then:  
    close    — close current connection  
    quit     — exit telnet client  
    status   — show connection status  
    ? or help — list telnet prompt commands

Send an explicit escape sequence to remote (if supported)  
    send ao    — send abort output  
    send ayt   — send "are you there?"  
    send ip    — send interrupt process  
    send eof   — send EOF

---

## Session management tips
Background the current telnet session (on some clients):  
    ~^Z
(Behavior depends on client and platform; `~` escapes are client-specific)

Resume or bring a backgrounded session to foreground using shell job controls (if applicable).

To pipe telnet output to a file (POSIX systems):  
    script telnet-output.txt
    telnet host.example.com
    exit
    exit   <-- stop `script` when finished

Log raw session with `tee` and `stdbuf` tricks (advanced; depends on OS and telnet client).

---

## Troubleshooting & diagnostics
If connection fails, check these in order:
    - Is the remote host reachable? `ping host.example.com`
    - Is the port open? `nc -vz host.example.com 23` or `nmap -p 23 host.example.com`
    - Is a firewall blocking port 23 (local or remote)?
    - Is the service expecting a different protocol (HTTP, SMTP, etc.) on that port?

If you see garbled output, try toggling local echo or switching modes (`mode line` / `mode character`).

---

## Security notes & best practice
- Telnet transmits everything (including passwords) in plain text — avoid for remote shell/login unless within an isolated/test lab or using an encrypted tunnel (e.g., SSH tunnel or VPN).  
- Prefer `ssh` for remote interactive shells:  
    ssh user@host.example.com
- If testing services that only speak plain-text protocols (e.g., SMTP, HTTP headers, custom TCP services), telnet (or `nc`) is useful for quick manual testing — but don't use telnet for real credentials across untrusted networks.

---

## Quick examples / combos
Check an SMTP server greeting on port 25:  
    telnet mail.example.com 25
(then type `HELO example.com` and press Enter)

Check an HTTP server response on port 80:  
    telnet web.example.com 80
    GET / HTTP/1.0
    Host: web.example.com

Test a custom TCP service on port 9000 and interact manually:  
    telnet 192.168.1.200 9000

Use netcat for scripting-friendly interactions (example send "ping" then exit):  
    printf "ping\r\n" | nc host.example.com 9000

---

## Handy references
- `man telnet` — platform-specific client options and behavior  
- Use `ssh` for secure shell access instead of telnet.  
- For automated testing of plain TCP services, prefer `nc` (netcat) or `socat`.

---

## Quick reminder
Telnet is great for quick plaintext TCP checks and testing protocol handshakes — **but do not use telnet for sensitive logins or over untrusted networks.**

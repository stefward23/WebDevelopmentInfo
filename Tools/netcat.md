# netcat (nc / ncat) — quick, copy-friendly Markdown cheat sheet

---

## Notes about implementations
- There are several implementations: **OpenBSD netcat**, **GNU netcat/traditional**, **ncat** (from Nmap), and BusyBox `nc`. Flags and behavior differ slightly (especially `-e`, `-q`, `-N`, `-k`, and SSL support).  
- If a flag below doesn't work, try the alternative form for your implementation or use `man nc` / `ncat --help`.

---

## Basic connect / listen

nc -lvnp <listening port> 

Connect to a host on port 80 (client mode):  
    nc example.com 80

Simple TCP server (listen) — OpenBSD style:  
    nc -l 1234

Simple TCP server (listen) — traditional GNU style:  
    nc -l -p 1234

Keep listening after client disconnect (GNU/OpenBSD differences):  
    # ncat (Nmap) keep-open option:
    ncat -l --keep-open -p 1234
    # OpenBSD/GNU often use a loop in shell:
    while true; do nc -l 1234 >/dev/null; done

Verbose output (show connection info):  
    nc -v example.com 80

Very verbose (some implementations):  
    nc -vv example.com 80

Numeric only (skip DNS):  
    nc -n 192.0.2.1 80

Timeouts:  
    # connection timeout (seconds)
    nc -w 5 example.com 80

Zero-I/O scan mode (port scan / probe):  
    nc -z -v example.com 22-25
    # For UDP probing add -u:
    nc -z -v -u example.com 53

---

## Transfer files
Send a file (server) and let client pull it:  
    # Server: send file
    nc -l 1234 < file.txt
    # Client: receive
    nc host.example.com 1234 > file.txt

With GNU `-q` to close after EOF (if supported):  
    nc -l -p 1234 -q 1 < file.bin
    # client:
    nc host 1234 > file.bin

OpenBSD style (no -q) example using `socat` or sleep loop if needed:
    while true

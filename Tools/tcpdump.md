# tcpdump — cheat sheet

man pcap-filter
---
## Example:
tcpdump -i eth0 -c 50 -v captures and displays 50 packets by listening on the eth0 interface, which is a wired Ethernet, and displays them verbosely.




## Basic capture

sudo tcpdump ip proto \\icmp -i <eth0>

Capture packets on the default interface (requires sudo/root):  
    sudo tcpdump

Show interfaces and exit:  
    tcpdump -D

Listen on a specific interface (example: eth0):  
    sudo tcpdump -i eth0

Listen on all interfaces:  
    sudo tcpdump -i any

Capture only N packets (example: 100):  
    sudo tcpdump -c 100 -i eth0

Run without name resolution (faster, show numeric IPs/ports):  
    sudo tcpdump -n -i eth0

Show packet timestamps with date (human readable):  
    sudo tcpdump -ttt -i eth0
(Use `-tt` for full timestamp with date/time.)

Show packet contents in ASCII and hex:  
    sudo tcpdump -X -i eth0

Show hex only:  
    sudo tcpdump -xx -i eth0

Print verbose packet info (one level) / very verbose:  
    sudo tcpdump -v -i eth0
    sudo tcpdump -vvv -i eth0

---

## Capture to file / read from file
Write capture to pcap file:  
    sudo tcpdump -w capture.pcap -i eth0

Append to an existing pcap (useful with rotation):  
    sudo tcpdump -W 10 -C 100 -w capture.pcap -i eth0
(See rotation example below.)

Read pcap file:  
    tcpdump -r capture.pcap

Read pcap and show ASCII payloads:  
    tcpdump -r capture.pcap -A

Filter while reading file (example: only port 80):  
    tcpdump -r capture.pcap 'port 80'

---

## Capture filters (BPF syntax)
Capture only traffic to/from host:  
    sudo tcpdump -i eth0 host 192.168.1.10

Source only:  
    sudo tcpdump -i eth0 src 192.168.1.10

Destination only:  
    sudo tcpdump -i eth0 dst 10.0.0.5

Match subnet (CIDR):  
    sudo tcpdump -i eth0 net 192.168.1.0/24

Match TCP/UDP/ICMP:  
    sudo tcpdump -i eth0 tcp
    sudo tcpdump -i eth0 udp
    sudo tcpdump -i eth0 icmp

Match a single port or port range:  
    sudo tcpdump -i eth0 port 443
    sudo tcpdump -i eth0 portrange 1024-2048

Combine expressions (use parentheses):  
    sudo tcpdump -i eth0 'tcp and (src 10.0.0.1 or dst port 22)'

Negation:  
    sudo tcpdump -i eth0 'not port 22'

Match multiple hosts:  
    sudo tcpdump -i eth0 'host 10.0.0.1 or host 10.0.0.2'

Capture HTTP traffic (port 80 & show payload):  
    sudo tcpdump -i eth0 -s 0 -A 'tcp port 80'

Capture only SYNs (useful for scanning detection):  
    sudo tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack == 0'

Capture small packets only (example length < 200 bytes):  
    sudo tcpdump -i eth0 'less 200'

Set snaplen (capture full packet payload):  
    sudo tcpdump -s 0 -i eth0   # capture full packet

Default snaplen is often 262144 (varies). Use `-s 0` to avoid truncation.

---

## Display options / formatting
Show numeric ports & IPs (no DNS reverse):  
    sudo tcpdump -n -i eth0

Show port numbers as names (default behavior — avoid `-n`):  
    sudo tcpdump -i eth0

Show link-level header in output (frame header):  
    sudo tcpdump -e -i eth0

Show timestamps as ISO date/time:  
    sudo tcpdump -tttt -i eth0

Limit output size / line length (for scripts):  
    sudo tcpdump -l -i eth0 | tee logfile
(`-l` makes stdout line-buffered; useful when piping.)

Print packet count and byte totals on exit:  
    sudo tcpdump -z -i eth0   # platform dependent; `-z` used for post-rotate command

---

## Capture rotation / ring buffer
Rotate files after reaching size (-C) and keep a limited number (-W):  
    sudo tcpdump -i eth0 -w /var/log/tcpdump/capture-%Y-%m-%d_%H:%M:%S.pcap -C 100 -W 10

Example common pattern (file basename with numeric rotation):  
    sudo tcpdump -i eth0 -w capture.pcap -C 100 -W 10

Notes:
- `-C` rotates at megabytes (100 => 100MB).  
- `-W` sets how many files to keep (ring).  
- On some systems use `-G` for time-based rotation plus `-w` with `strftime` format:
    sudo tcpdump -i eth0 -G 3600 -w 'capture-%Y%m%d%H%M%S.pcap' -W 24

---

## Performance & safety
Run as non-root where possible (may restrict capture capabilities) — typically `sudo` required.  
Use `-s 0` to capture full packets but this increases disk and CPU.  
Apply capture filters to reduce volume (BPF filters run in kernel, efficient).  
Prefer `dumpcap` (from Wireshark) for high-volume capture — it's optimized and drops privileges after opening the interface:
    sudo dumpcap -i eth0 -w capture.pcap

Watch disk space when writing pcaps — rotated captures prevent single-file bloat.

---

## Common troubleshooting / checks
No packets captured?
    - Is interface correct? `tcpdump -D` to list interfaces.
    - Are you running as root/sudo? `sudo tcpdump -i eth0`
    - Is there a capture filter that excludes traffic? Remove filter to confirm.
    - Is the interface in promiscuous mode? `-p` disables promiscuous; by default tcpdump enables promiscuous.
    - On VM hosts, check host vs guest interface mapping.

Permissions/SELinux problems:
    - Ensure user has capabilities (Linux `CAP_NET_RAW` / `CAP_NET_ADMIN`) or run with sudo.
    - Check `audit`/SELinux logs if capture fails unexpectedly.

---

## Examples / combos
Capture DNS queries (UDP 53) and show payload:  
    sudo tcpdump -i any -n -s 0 -vvv -A 'udp port 53'

Capture HTTPS handshake (SYN/ACK) only and write pcap:  
    sudo tcpdump -i eth0 -n 'tcp port 443 and (tcp[tcpflags] & (tcp-syn|tcp-ack) != 0)' -w https-handshake.pcap

Capture SSH traffic to/from host 10.0.0.5, show hex:  
    sudo tcpdump -i eth0 -n -s 0 -xx 'host 10.0.0.5 and port 22' -c 200

Follow a TCP stream-ish (not full reassembly — use tshark/wireshark for reassemble):  
    sudo tcpdump -s 0 -A -n 'tcp and host 10.0.0.5 and port 80'

Read pcap and filter by time range (use `tcpdump -r` then additional filter, or use `editcap`/`tshark` for advanced time slicing):
    tcpdump -r capture.pcap 'tcp and port 80 and (not src net 10.0.0.0/8)'

---

## Integration with Wireshark/tshark/dumpcap
Open pcap in Wireshark:  
    wireshark capture.pcap &

Use `tshark` (CLI Wireshark) for more advanced field extraction:  
    tshark -r capture.pcap -Y 'http' -T fields -e ip.src -e http.host -e http.request.uri

Use `editcap` to split/cut pcap files by time or packet count:  
    editcap -c 1000 large.pcap part.pcap

## Advanced 
tcp-syn TCP SYN (Synchronize)  
tcp-ack TCP ACK (Acknowledge)  
tcp-fin TCP FIN (Finish)  
tcp-rst TCP RST (Reset)  
tcp-push TCP Push  
Based on the above, we can write:  

tcpdump "tcp[tcpflags] == tcp-syn" to capture TCP packets with only the SYN (Synchronize) flag set, while all the other flags are unset.  
tcpdump "tcp[tcpflags] & tcp-syn != 0" to capture TCP packets with at least the SYN (Synchronize) flag set.  
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0" to capture TCP packets with at least the SYN (Synchronize) or ACK (Acknowledge) flags set.  

## Helpful BPF snippets
HTTP requests:  
    'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

ARP traffic:  
    arp

Broadcast traffic:  
    broadcast

Multicast traffic:  
    multicast

Only packets larger than X bytes (example > 1500):  
    'greater 1500'

---
  
tcpdump host IP or tcpdump host HOSTNAME	Filters packets by IP address or hostname  
tcpdump src host IP or	Filters packets by a specific source host  
tcpdump dst host IP	Filters packets by a specific destination host  
tcpdump port PORT_NUMBER	Filters packets by port number  
tcpdump src port PORT_NUMBER	Filters packets by the specified source port number  
tcpdump dst port PORT_NUMBER	Filters packets by the specified destination port number  
tcpdump PROTOCOL	Filters packets by protocol; examples include ip, ip6, and icmp  
  
## Quick reference (one-liners)
Capture 1,000 packets on eth0, numeric output, full packets, save to file:  
    sudo tcpdump -i eth0 -c 1000 -n -s 0 -w capture.pcap

Read and print ASCII payloads from pcap:  
    tcpdump -r capture.pcap -A

Rotate files every hour, keep 24 files:  
    sudo tcpdump -i eth0 -G 3600 -w 'capture-%Y%m%d%H%M%S.pcap' -W 24 -n -s 0

Capture only traffic to/from 192.168.1.5 and port 22:  
    sudo tcpdump -i any -n 'host 192.168.1.5 and port 22'

List interfaces:  
    tcpdump -D

---


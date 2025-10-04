# enum4linux — quick, copy-friendly Markdown cheat sheet

Enum4linux is a Perl wrapper around Samba tools (smbclient, rpcclient, net, nmblookup) for enumerating Windows/Samba hosts (users, groups, shares, OS info, password policy, etc.). 

---

## Navigating SMB
# List available shares on a host
smbclient -L //<IP> -U <USER>

# Connect to a share (you'll get an smbclient prompt)
smbclient //<IP>/<SHARE> -U <USER>
# then in the smbclient prompt:
ls                # list files in the current remote dir
dir               # same as ls
cd <remote-dir>   # change remote directory
pwd               # show remote working directory
lcd <local-dir>   # change local directory (where downloads go)
lpwd              # show local directory
get <file>        # download single file
mget <pattern>    # download multiple files (wildcards)
put <file>        # upload single file
mput <pattern>    # upload multiple files
del <file>        # delete remote file
rm <file>         # alias for del
mkdir <dir>       # create remote directory
rmdir <dir>       # remove remote directory (if empty)
recurse ON|OFF    # enable/disable recursive mget/mput
prompt            # toggle interactive prompting for mget/mput
mask <pattern>    # set wildcard mask for mget/mput
stat <file>       # show file info
help              # list smbclient commands
quit / exit       # leave smbclient


## SMB client connect
Example: smbclient //10.10.10.10/secrets -U Anonymous -p 445

---

## Quick reference (one-liners)
Do-everything scan:
    enum4linux -a 192.168.1.100

List users (anonymous):
    enum4linux -U 192.168.1.100

List shares:
    enum4linux -S 192.168.1.100

Use creds:
    enum4linux -u user -p pass 192.168.1.100

Verbose:
    enum4linux -v 192.168.1.100

---

## Basic usage
Run enum4linux against a single IP:
    enum4linux 192.168.1.100

Show help / usage:
    enum4linux -h

Run verbose:
    enum4linux -v 192.168.1.100

Run the “do-everything” quick scan (common full enum):
    enum4linux -a 192.168.1.100
(“-a” runs a set of useful checks: SMB shares, users, groups, OS, policies, etc.). :contentReference[oaicite:2]{index=2}

---

## Useful flags (common & copy-friendly)
List users (enumerate user accounts via RPC/SMB):
    enum4linux -U 192.168.1.100

List groups:
    enum4linux -G 192.168.1.100

List shares:
    enum4linux -S 192.168.1.100

Get OS and machine info:
    enum4linux -o 192.168.1.100

SNMP/RID cycling / user RID queries (useful for older Windows RID leaks):
    enum4linux -r 192.168.1.100

Password policy info:
    enum4linux -P 192.168.1.100

Get all available info (equivalent to many flags):
    enum4linux -a 192.168.1.100

Use supplied credentials (username + password):
    enum4linux -u Administrator -p 'Passw0rd!' 192.168.1.100

Use different domain and credentials:
    enum4linux -u 'DOMAIN\\user' -p 'pass' 192.168.1.100

Run with proxychains (if routing through a proxy):
    proxychains enum4linux -a 192.168.1.100

Notes: exact output/coverage depends on target SMB config and Windows version. :contentReference[oaicite:3]{index=3}

---

## Quick examples / combos
Verbose full enumeration:
    enum4linux -v -a 192.168.1.100

Get shares and try anonymous SMB:
    enum4linux -S -o 192.168.1.100

Enumerate with creds (dump users/groups with provided account):
    enum4linux -u svc_account -p 'P@ss1234' -a 192.168.1.100

RID cycling + user listing:
    enum4linux -r -U 192.168.1.100

Scripted scan of multiple targets from a file (bash loop):
    for ip in $(cat hosts.txt); do enum4linux -a $ip > results_$ip.txt; done

---

## Interpreting output — what to look for
- **Users & groups**: user accounts that can be targeted for password guessing or pivoting.  
- **Shares**: readable shares may contain creds, config files, or sensitive data; writable shares could be used for persistence.  
- **Password policy & domain info**: helps plan safe, rule-abiding tests (lockout thresholds, complexity).  
- **OS/version strings & SMB dialects**: may reveal outdated/known-vulnerable versions. :contentReference[oaicite:4]{index=4}

---

## When enum4linux fails / troubleshooting
- Ensure ports 139/445 are reachable (use `nmap -p 139,445 target`).  
- Some modern Windows hosts/AD DCs restrict anonymous enumeration or SMBv1; supplying valid credentials may be required.  
- Firewalls or SMB signing/NTLMv2-only settings can block or limit responses.  
- If output is empty, try `smbclient -L //target -N` or `rpcclient` directly to confirm behavior (enum4linux is a wrapper and depends on underlying Samba tools). :contentReference[oaicite:5]{index=5}

---

## Alternatives & modern variants
- **enum4linux-ng / Enum4LinuxPy** — community rewrites with JSON/YAML outputs and extra features (check GitHub). Consider them for richer output or active development. :contentReference[oaicite:6]{index=6}

---

## Best practices & legal / safety notes
- Always have explicit authorization before scanning/enumerating any host. Unauthorized scanning can be illegal and disruptive.  
- For defensive use, run tests in a lab or on scheduled windows and coordinate with system owners.  
- Combine enum4linux output with `nmap` SMB scripts (e.g., `nmap --script smb-enum-shares,smb-os-discovery -p 139,445 target`) for corroboration and deeper discovery. :contentReference[oaicite:7]{index=7}

---

## Quick reference (one-liners)
Do-everything scan:
    enum4linux -a 192.168.1.100

List users (anonymous):
    enum4linux -U 192.168.1.100

List shares:
    enum4linux -S 192.168.1.100

Use creds:
    enum4linux -u user -p pass 192.168.1.100

Verbose:
    enum4linux -v 192.168.1.100

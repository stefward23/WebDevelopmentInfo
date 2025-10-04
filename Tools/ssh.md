# SSH — cheat sheet

---

## Basic connections
Connect to a host (default port 22):  
    ssh user@host.example.com

Connect to a host on a different port:  
    ssh -p 2222 user@host.example.com

Use a specific private key file:  
    ssh -i ~/.ssh/id_rsa user@host.example.com

Connect by IP address:  
    ssh admin@192.168.1.10

Jump through a bastion (ProxyJump) — modern & simple:  
    ssh -J jumpuser@bastion.example.com targetuser@target.internal

Old-style ProxyCommand (netcat on bastion):  
    ssh -o 'ProxyCommand ssh -W %h:%p jumpuser@bastion.example.com' targetuser@target.internal

---

## Authentication / key management
Generate an RSA keypair (default 3072 bits) interactively:  
    ssh-keygen

Generate an ed25519 keypair (recommended modern default):  
    ssh-keygen -t ed25519

Generate with a custom filename and no passphrase (not recommended):  
    ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_example -N ""

Copy your public key to a remote account (password-based first):  
    ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host.example.com

Add key to agent (so you don't re-enter passphrase):  
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_ed25519

List keys currently loaded in agent:  
    ssh-add -l

Remove all keys from agent:  
    ssh-add -D

Enable agent forwarding (use cautiously — security risk):  
    ssh -A user@host.example.com

---

## Command execution & file transfer
Run a command on remote host and exit:  
    ssh user@host.example.com 'sudo apt update && sudo apt upgrade -y'

Copy file to remote using scp:  
    scp ./localfile.txt user@host.example.com:/remote/path/

Copy directory recursively with scp:  
    scp -r ./localdir user@host.example.com:/remote/path/

Use rsync over SSH (efficient sync):  
    rsync -avz -e "ssh -p 2222" ./localdir/ user@host.example.com:/remote/path/

Use sftp interactive file transfer:  
    sftp user@host.example.com

Use single-file transfer with sftp batch:  
    sftp user@host.example.com <<< $'put localfile.txt /remote/path/localfile.txt\nbye'

Run a local command and pipe to remote ssh (stdin):  
    cat localfile | ssh user@host.example.com 'cat > /remote/path/localfile'

---

## Tunneling / port forwarding
Local port forward (client -> remote host):  
    ssh -L 8080:internal.service:80 user@bastion.example.com
Access http://localhost:8080 -> internal.service:80 via bastion.

Remote port forward (remote -> client):  
    ssh -R 2222:localhost:22 user@public.example.com
Allows someone on public.example.com to connect to your local SSH via port 2222.

Dynamic SOCKS proxy (useful for proxying browser traffic):  
    ssh -D 1080 -C -N user@bastion.example.com
Then configure browser to use SOCKS5 localhost:1080.

Background a tunnel (-N: no remote command, -f: go to background):  
    ssh -f -N -L 3306:db.internal:3306 user@bastion.example.com

Reverse dynamic (experimental) — not common; prefer explicit forwards.

---

## Multiplexing & performance
Use connection multiplexing in `~/.ssh/config` for faster repeated connections:

    Host *.internal
      User youruser
      ControlMaster auto
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlPersist 10m

Disable pseudo-tty allocation for non-interactive commands:  
    ssh -T user@host.example.com 'git-receive-hook'

Enable compression (useful on slow links):  
    ssh -C user@host.example.com

Set keepalive to avoid session drop (`ServerAliveInterval` seconds):  
    ssh -o ServerAliveInterval=60 user@host.example.com

---

## SSH client options (common)
Specify identity file:  
    -i /path/to/key

Specify port:  
    -p PORT

Run in background (no remote command):  
    -f -N

Disable pseudo-tty allocation:  
    -T

Force pseudo-tty (for interactive scripts):  
    -t

Forward X11 (needs X server on client and server support):  
    -X    # trusted
    -Y    # trusted (less restrictive on some servers)

Verbose debug output (useful for troubleshooting):  
    ssh -v user@host.example.com
    ssh -vvv user@host.example.com   # very verbose

Set config option inline:  
    ssh -o StrictHostKeyChecking=no user@host.example.com

Use ProxyJump inline:  
    ssh -J jump@bastion target@internal

Limit ciphers / key exchange algorithms (compatibility/requirements):  
    ssh -o Ciphers=aes128-ctr -o KexAlgorithms=diffie-hellman-group14-sha1 user@host

---

## `~/.ssh/config` — handy examples
Add multiple host shortcuts and per-host options:

    Host bastion
      HostName bastion.example.com
      User jumpuser
      IdentityFile ~/.ssh/id_ed25519

    Host internal-*
      User admin
      ProxyJump bastion
      Port 22
      ForwardAgent yes

Now you can `ssh internal-db1` instead of long commands.

---

## Key-based vs password-based & security
- Prefer key-based auth (ed25519 / rsa 3072+ if needed) and disable password auth on servers:  
    In `/etc/ssh/sshd_config`: `PasswordAuthentication no`, then `systemctl reload sshd`.
- Disable root login: `PermitRootLogin no`.
- Use `AllowUsers` / `AllowGroups` to restrict who may connect.
- Use fail2ban or rate-limiting firewall rules to reduce brute-force attempts.
- Don’t enable `ForwardAgent` unless you trust the remote host.
- Use `ssh-keyscan` to collect host keys for known_hosts bootstrapping (careful: MITM risk until verified).

Add host key to known_hosts non-interactively (example):  
    ssh-keyscan -H host.example.com >> ~/.ssh/known_hosts

Verify host key fingerprint out-of-band before trusting it.

---

## Troubleshooting
Common checks when connection fails:
    - Is SSH server running? `sudo systemctl status sshd` on server.
    - Is port reachable? `nc -vz host.example.com 22` or `telnet host.example.com 22`
    - Firewall rules (ufw/iptables/security groups) blocking port?
    - Check server logs: `/var/log/auth.log` or `journalctl -u sshd`.
    - Use `ssh -vvv` to debug authentication/key issues and see which key is attempted.
    - Permissions on key files: private key must be `chmod 600 ~/.ssh/id_*` and `~/.ssh` `700`.
    - SELinux or AppArmor may affect sshd behavior on some distros.

---

## Advanced / lesser-known tricks
SSHFS — mount remote filesystem over SSH:  
    sshfs user@host:/remote/path ~/mnt/host

ControlMaster single connection for multiple sessions:  
    ssh -M -S /tmp/ctrl-socket -fN user@host    # start master
    ssh -S /tmp/ctrl-socket user@host           # reuse

Forward X11 and enable trusted X11 forwarding if needed:  
    ssh -Y -X user@host.example.com

Use `ProxyCommand` with `nc.openbsd` for SOCKS-friendly proxying:  
    Host target
      ProxyCommand ssh -W %h:%p jumpuser@bastion

Port knocking (sequence to open firewall) — depends on server setup; example client tool `knock`.

Two-factor & hardware tokens — many servers integrate with PAM or use `authenticator` methods; check server config.

---

## Examples / combos
Connect with key, run remote upgrade, and exit:  
    ssh -i ~/.ssh/id_ed25519 user@host 'sudo apt update && sudo apt upgrade -y'

Create a local SOCKS proxy through a bastion and background it:  
    ssh -f -N -D 1080 user@bastion.example.com

Tunnel a remote DB port to local machine:  
    ssh -L 5432:db.internal:5432 user@bastion.example.com

Sync code with rsync over SSH, preserve perms:  
    rsync -avz -e "ssh -i ~/.ssh/id_ed25519 -p 2222" ./project/ user@host:/var/www/project/

Use SSH to execute a remote script with input (here-document):  
    ssh user@host 'bash -s' <<'EOF'
    echo "Running remote script"
    uname -a
    EOF

---

## Quick reference (one-liners)
Basic connect:  
    ssh user@host

Connect with key:  
    ssh -i ~/.ssh/id_ed25519 user@host

Copy public key to remote:  
    ssh-copy-id user@host

Start local SOCKS proxy:  
    ssh -D 1080 -C -N user@bastion

Remote port forward:  
    ssh -R 2222:localhost:22 user@public.example.com

Verbose debug:  
    ssh -vvv user@host

Generate ed25519 key:  
    ssh-keygen -t ed25519

---

## Notes & best practices
- Use key-based, passphrase-protected keys + `ssh-agent`. Prefer `ed25519`.  
- Limit exposure: change default port only as obscurity measure, but rely on proper firewall and rate-limiting.  
- Keep your `~/.ssh/config` to simplify repeated connections and set safe defaults.  
- Regularly rotate keys and remove unused public keys from `~/.ssh/authorized_keys`.  
- Audit `authorized_keys` for suspicious options (e.g., `command="..."` can be used for restricted keys).  
- Never paste private keys into shared systems or send them over untrusted channels.

---

If you want a **plain-text (no Markdown)** single-block version or want me to add an `ssh` troubleshooting checklist or example `~/.ssh/config` tuned for bastion + internal hosts, say “plain please” or “add config”.  

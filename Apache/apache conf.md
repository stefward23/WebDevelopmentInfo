# Updates repositories and fetches information about available updates
```
sudo apt update
```
# Upgrades all installed packages to their latest versions
```
sudo apt upgrade
```
# Installs Apache web server
```
sudo apt install apache2 -y
```
# Displays the IP address of the system
```
hostname -I
```
# Open the web browser and go to the server IP (replace <ip_address> with actual IP)
```
http://<ip_address>
```
# Adds the user 'pi' to the 'www-data' group, allowing web server access
```
sudo usermod -a -G www-data pi
```
# Changes ownership of the files in /var/www/html to 'www-data' (Apache user/group)
```
sudo chown -R -f www-data:www-data /var/www/html
```
# Edits the index.html file in /var/www/html (Apache default document root)
```
sudo nano /var/www/html/index.html
```

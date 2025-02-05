### Update repository and fetch information about available updates
```
sudo apt update
```
### Upgrade all installed packages to their latest versions
```
sudo apt upgrade
```
### Install Apache web server
```
sudo apt install apache2 -y
```
### Display the IP address of the system
```
hostname -I
```
### Open the web browser and go to the server IP (replace <ip_address> with actual IP)
```
http://<ip_address>
```
### Add the user 'pi' to the 'www-data' group, allowing web server access
```
sudo usermod -a -G www-data pi
```
### Change ownership of the files in /var/www/html to 'www-data' (Apache user/group)
```
sudo chown -R -f www-data:www-data /var/www/html
```
### Edit the index.html file in /var/www/html (Apache default document root)
```
sudo nano /var/www/html/index.html
```

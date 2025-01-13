sudo apt update

sudo apt upgrade

sudo apt install apache2 -y

hostname -I

http://<ip_address>

sudo usermod -a -G www-data pi

sudo chown -R -f www-data:www-data /var/www/html

sudo nano /var/www/html/index.html

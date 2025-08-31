# Update system packages
sudo apt update 

sudo apt upgrade -y

# Install apache2
sudo apt install apache2 -y

# Adjust firewall (if UFW is enabled)
sudo ufw allow 'Apache'

sudo ufw enable      # only if not already enabled

sudo ufw status

# Start and enable apache
sudo systemctl start apache2

sudo systemctl enable apache2

# Check apache status:
systemctl status apache2

# Test installation
http://<your_server_ip>

# Create a directory for your site:
sudo mkdir -p /var/www/example.com/html

sudo chown -R $USER:$USER /var/www/example.com/html

# Create a sample HTML file:
nano /var/www/example.com/html/index.html

#### Add
<html>
  <head><title>Welcome</title></head>
  <body><h1>Apache on Ubuntu Works!</h1></body>
</html>

# Create a virtual host config
sudo nano /etc/apache2/sites-available/example.com.conf

#### Add
<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# Enable new site and reload apache
sudo a2ensite example.com.conf
sudo a2dissite 000-default.conf
sudo systemctl reload apache2

# Enable SSL with Let’s Encrypt

#### Install Certbot:
sudo apt install certbot python3-certbot-apache -y


#### Run:
sudo certbot --apache
This will configure HTTPS automatically.

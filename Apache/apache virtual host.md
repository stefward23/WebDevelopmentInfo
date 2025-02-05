### Opens the Apache site configuration file for 'example.com'
```
sudo nano /etc/apache2/sites-available/example.com.conf
```
### Example configuration for the site 'example.com'
```
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```
### Creates the directory structure for the site files
```
sudo mkdir -p /var/www/example.com/public_html
```
### Changes ownership of the site's directory to the Apache user/group (www-data)
```
sudo chown -R www-data:www-data /var/www/example.com/public_html
```
### Enables the 'example.com.conf' site configuration
```
sudo a2ensite example.com.conf
```
### Reloads Apache to apply the changes made to the site configuration
```
sudo systemctl reload apache2
```

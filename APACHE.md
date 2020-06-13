```sh
sudo nano /etc/apache2/sites-available/your_domain.xyz.conf
```
```
<VirtualHost *:80>
    ServerAdmin admin@your_domain.xyz
    ServerName your_domain.xyz
    ServerAlias www.your_domain.xyz
    DocumentRoot /var/www/your_domain.xyz/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```sh
sudo a2ensite your_domain.xyz.conf

sudo systemctl restart apache2
```

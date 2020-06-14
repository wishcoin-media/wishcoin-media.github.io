# Configuring apache server to serve your domain

```sh
sudo nano /etc/apache2/sites-available/your_domain.com.conf
```


```
<VirtualHost *:80>
    ServerAdmin admin@your_domain.com
    ServerName your_domain.com
    ServerAlias www.your_domain.com
    Redirect / https://your_domain.com/
    ServerAlias www.your_domain.com
    DocumentRoot /var/www/your_domain.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```sh
sudo a2ensite your_domain.com.conf
sudo systemctl restart apache2
```
## Go to the ssl config file

```sh
sudo nano /etc/apache2/sites-available/your_domain.com-le-ssl.conf
```

## Add this below the CustomLog

```
<If "%{HTTP_HOST} != 'your_domain.com'">
    Redirect "/" "https://your_domain.com/"
</If>
```

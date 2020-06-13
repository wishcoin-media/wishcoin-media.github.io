# Setting up Directus on your $5 Lightsail Server

## Update Package Manager

```sh
sudo apt update
```

## Install LAMP stack

```sh
sudo apt install apache2
sudo apt install mysql-server
sudo mysql_secure_installation //optional
sudo apt install php libapache2-mod-php php-pdo php-mysql php-curl php-gd php-fileinfo php-mbstring php-xml
```

## Update file permissions

```sh
sudo chmod -R 755 /var/www
sudo chown -R ubuntu /var/www
sudo chown -R www-data:www-data /var/www/directus
```

## Install git and clone directus

```sh
sudo apt install git
git clone https://github.com/directus/directus.git /var/www/directus
```

#### Enable rewrite

```sh
sudo a2enmod rewrite
sudo systemctl reload apache2
```

## Configure apache to host directus

```sh
sudo nano /etc/apache2/sites-available/directus.conf
```

```httpd
<VirtualHost \*:80>
ServerAdmin webmaster@localhost
ServerName directus.jitendrainfra.com
DocumentRoot /var/www/directus/public

        <Directory /var/www/directus/public/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        <IfModule mod_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

</VirtualHost>
```

```sh
sudo a2ensite directus.conf
sudo a2dissite 000-default.conf

sudo rm /etc/apache2/sites-available/000-default.conf
sudo rm /etc/apache2/sites-available/default-ssl.conf

sudo systemctl reload apache2
```

## Configure mysql

```sh
sudo chown -R mysql:mysql /var/lib/mysql

sudo mysql
```

```sql
CREATE USER 'directus'@'localhost' IDENTIFIED BY 'OSL@123*';
GRANT ALL PRIVILEGES ON * . \* TO 'directus'@'localhost';
FLUSH PRIVILEGES;
CREATE DATABASE cms;
\q
```

## Setup https and allow it through firewall

```sh
sudo apt install certbot python3-certbot-apache
sudo certbot --apache
```

- Go to your website
- start the setup
- get the password

## Post Setup

```sh
mysql -u directus -p
```

```sql
USE cms;
UPDATE directus_users SET token = SECRET_TOKEN WHERE first_name = COMPANY_NAME
```

# UVdesk Installation on Ubuntu Server

## Upgrade Ubuntu
```sh
sudo apt update
sudo apt upgrade -y
```

## Install Packages
```sh
sudo apt install curl wget zip composer apache2 mariadb-server mariadb-client php php-curl php-intl php-gd php-dom php-iconv php-xsl php-mbstring php-ctype php-zip php-pdo php-xml php-bz2 php-calendar php-exif php-fileinfo php-json php-mysqli php-mysql php-posix php-tokenizer php-xmlwriter php-xmlreader php-phar php-soap php-mysql php-fpm libapache2-mod-php php-gmp php-bcmath php-apcu php-redis php-imagick php-imap php-xdebug php-mailparse -y
```

## Become Super User
```sh
sudo su
```
## Configure DB
Enter current password for root (enter for none)<br>
Change the root password? [Y/n] n<br>
Remove anonymous users? [Y/n] Y<br>
Disallow root login remotely? [Y/n] Y<br>
Remove test database and access to it? [Y/n] Y<br>
Reload privilege tables now? [Y/n] Y<br>

```sh
mysql -u root -p
```
## Create DB
```sql
CREATE DATABASE uvdesk;
GRANT ALL ON uvdesk.* to 'YOUR_DB_USER'@'localhost' IDENTIFIED BY 'YOUR_DB_USER_PASSWORD';
FLUSH PRIVILEGES;
EXIT;
```

## Exit Super User
```sh
exit
```

## Download UVdesk Latest Release
```sh
wget -O uvdesk.zip https://cdn.uvdesk.com/uvdesk/downloads/opensource/uvdesk-community-current-stable.zip
sudo unzip ./uvdesk.zip -d /var/www/
sudo mv /var/www/uvdesk* /var/www/uvdesk
sudo chown -R www-data:www-data /var/www/uvdesk
sudo nano /etc/apache2/sites-available/uvdesk.conf
```

## Configure uvdesk.conf
```configuration
Alias /uvdesk "/var/www/uvdesk/public"
<Directory /var/www/uvdesk/public>
Require all granted
Options Indexes FollowSymLinks
AllowOverride All
Order allow,deny
Allow from all
</Directory>
```

## Configure Services
```sh
sudo a2enmod rewrite
sudo a2ensite uvdesk
sudo systemctl reload apache2
sudo systemctl restart apache2
```


## Configure UVdesk
Open in browser http://YOUR_DNS_OR_YOUR_IP/uvdesk
Click the "Let's Begin" and then "Proceed" <br>
Complete the Database Configuration <br>

```configuration
Server: 127.0.0.1
Port: 3306
Username: YOUR_DB_USER
Password: YOUR_DB_USER_PASSWORD
Database: uvdesk
```
Create UVdesk Admin Account and then "Proceed" <br>
Complete configuration by clicking "Proceed" button at the Website Configuration page <br>
Click Install Now <br>
After the installation completes, click the Admin Panel button or http://YOUR_DNS_OR_YOUR_IP/uvdesk/en/member/dashboard

## Apache2 Configuration
```sh
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
Change DocumentRoot /var/www/uvdesk/public <br>
```sh
sudo systemctl reload apache2
```

# Server Setup -  Lamp Installation

**Step 1 — Installing Apache and Updating the Firewall**

Update the system

<code>sudo apt update</code>

# install apache
sudo apt install apache2

sudo systemctl status apache2

sudo ufw app list

sudo ufw allow in "Apache"

sudo ufw status

http://your_server_ip

Enable Mod-Rewrite
# enable rewrite
sudo a2enmod rewrite
# restart apache
sudo service apache2 restart

# Enabling .htaccess file to rewrite path
sudo nano /etc/apache2/apache2.conf
#AllowOverride None change None to All

<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

sudo service apache2 restart


 
**Step 2 — Installing PHP**

sudo apt install --no-install-recommends php8.1

php -v

#install default packages
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath

sudo service apache2 restart


Step 3 — Installing MySQL

sudo apt install mysql-server

sudo nano /etc/mysql/my.cnf
[mysqld]
sql-mode="STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"



sudo systemctl start mysql.service

sudo mysql_secure_installation 

sudo mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'OAtEtime';

exit

Step 4 — Installing PHPMyAdmin

sudo apt install phpmyadmin

sudo phpenmod mbstring

sudo service apache2 restart


sudo nano /etc/apache2/apache2.conf

Include /etc/phpmyadmin/apache.conf

sudo service apache2 restart

Step 5 — Setup the permission group

# Set group to www-data
sudo chgrp www-data /var/www/html
# Make it writable for the group
sudo chmod 775 /var/www/html
# Set GID to www-data for all sub-folders
sudo chmod g+s /var/www/html
# Add your username to www-data group
sudo usermod -a -G www-data ubuntu
# Finally change ownership to 
sudo chown ubuntu /var/www/html
# Your account shouldn't have any more permission issues

Step 6 — How To Add Swap Space

sudo fallocate -l 2G /swapfile
ls -lh /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab



Step 7 — Install Composer

cd ~
curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/local/bin/composer



Setup VirtualHost 

sudo nano /etc/apache2/sites-available/your-domain.conf

#Add the following content:

<VirtualHost *:80>

ServerAdmin webmaster@your-domain.com

ServerName your-domain.com
ServerAlias www.your-domain.com
DocumentRoot /var/www/html/

<Directory /var/www/html/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>

ErrorLog ${APACHE_LOG_DIR}/your-domain.com_error.log
CustomLog ${APACHE_LOG_DIR}/your-domain.com_access.log combined

</VirtualHost>

#Enable the Apache virtual host:
sudo a2ensite your-domain.conf

#After that, restart the Apache web server
sudo service apache2 restart

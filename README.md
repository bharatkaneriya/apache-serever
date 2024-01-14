# Server Setup -  Lamp Installation

**Step 1 — Installing Apache and Updating the Firewall**

Update the system

<code>sudo apt update</code>

install apache

<code>sudo apt install apache2</code>

<code>sudo systemctl status apache2</code>

<code>sudo ufw app list</code>

<code>sudo ufw allow in "Apache"</code>

<code>sudo ufw status</code>

Open the link below in your browser to test your installation
http://your_server_ip

Enable Mod-Rewrite

<code>sudo a2enmod rewrite</code>

<code>sudo service apache2 restart</code>

Enabling .htaccess file to rewrite path

Open apache2.conf and change AllowOverride None change None to All

<code>sudo nano /etc/apache2/apache2.conf</code>

<code>
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
</code>

<code>sudo service apache2 restart</code>


 
**Step 2 — Installing PHP**

<code>sudo apt install --no-install-recommends php8.1</code>

To test installation run below command in your terminal.

<code>php -v</code>

install default packages

<code>sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath</code>

<code>sudo service apache2 restart</code>


**Step 3 — Installing MySQL**

<code>sudo apt install mysql-server</code>

<code>sudo nano /etc/mysql/my.cnf</code>

<code>
[mysqld]
sql-mode="STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"
</code>code>

<code>sudo systemctl start mysql.service</code>

<code>sudo mysql_secure_installation</code> 

<code>sudo mysql</code>

<code>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Password';</code>

<code>exit</code>

**Step 4 — Installing PHPMyAdmin**

<code>sudo apt install phpmyadmin</code>

<code>sudo phpenmod mbstring</code>

<code>sudo service apache2 restart</code>

<code>sudo nano /etc/apache2/apache2.conf</code>

Add code below to apache2.conf for include phpmyadmin

<code>Include /etc/phpmyadmin/apache.conf</code>

<code>sudo service apache2 restart</code>


**Step 5 — Setup the permission group**

Set group to www-data

<code>sudo chgrp www-data /var/www/html</code>

Make it writable for the group

<code>sudo chmod 775 /var/www/html</code>

Set GID to www-data for all sub-folders

<code>sudo chmod g+s /var/www/html</code>

Add your username to www-data group

<code>sudo usermod -a -G www-data ubuntu</code>

Finally, change ownership to 

<code>sudo chown ubuntu /var/www/html</code>


**Step 6 — How To Add Swap Space**

<code>
sudo fallocate -l 2G /swapfile
ls -lh /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
</code>


**Step 7 — Install Composer**

<code>
cd ~
curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/local/bin/composer
</code>

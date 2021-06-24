# Install Wordpress with nginx in ubuntu 20.04 <!-- omit in toc -->

- [Step 1: install nginx](#step-1-install-nginx)
- [Step 2: configure nginx for php](#step-2-configure-nginx-for-php)
- [Step 3: install php 8.0](#step-3-install-php-80)
- [Step 4: install mysql](#step-4-install-mysql)
- [Step 5: create a database & user in mysql](#step-5-create-a-database--user-in-mysql)
- [Step 5: install phpmyadmin 5 & configure](#step-5-install-phpmyadmin-5--configure)
- [Step 6: install wordpress](#step-6-install-wordpress)
- [Step 7: change wordpress configuration](#step-7-change-wordpress-configuration)
- [Step 8: restart nginx server](#step-8-restart-nginx-server)

# Step 1: install nginx
```sh
sudo apt install nginx
```

# Step 2: configure nginx for php
```sh
sudo gedit /etc/nginx/sites-available/default
```

add this lines:
```txt
# Add index.php to the list if you are using PHP
index index.php index.html index.htm index.nginx-debian.html;

location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.0-fpm.sock;
}
```

# Step 3: install php 8.0
```sh
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php php8.0-fpm php8.0-zip php-json php-xml php8.0-mbstring php8.0-mysql
```

# Step 4: install mysql
```sh
sudo apt install mysql-server mysql-client
```

# Step 5: create a database & user in mysql
```sh
mysql -u root -p
```

mysql console:
```sql
CREATE DATABASE wp;
SHOW DATABASES;
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

# Step 5: install phpmyadmin 5 & configure
```sh
curl -O https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
unzip phpMyAdmin-5.1.1-all-languages.zip
sudo mv phpMyAdmin-5.1.1-all-languages /usr/share/phpmyadmin
sudo mkdir /usr/share/phpmyadmin/tmp
sudo chown -R www-data:www-data /usr/share/phpmyadmin
sudo chmod 777 /usr/share/phpmyadmin/tmp
sudo ln -s /usr/share/phpmyadmin /var/www/html
sudo chown www-data:www-data /var/www/html
```

# Step 6: install wordpress
```sh
curl -O https://wordpress.org/latest.zip
unzip latest.zip
sudo mv wordpress /var/www/html
sudo mv /var/www/html/wordpress /var/www/html/wp
```

# Step 7: change wordpress configuration
```sh
sudo chown www-data:www-data /var/www/html/wp
```

# Step 8: restart nginx server
```sh
service nginx restart
```
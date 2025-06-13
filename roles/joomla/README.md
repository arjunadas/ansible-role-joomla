```
apt install nginx mysql-server unzip nano -y

apt install php8.1 php8.1-common php8.1-cli php8.1-fpm php8.1-mysql php8.1-opcache php8.1-gmp php8.1-curl php8.1-intl php8.1-mbstring php8.1-xmlrpc php8.1-gd php8.1-xml php8.1-zip -y


wget https://github.com/joomla/joomla-cms/releases/download/5.1.4/Joomla_5.1.4-Stable-Full_Package.zip -O joomla.zip

mkdir -p /var/www/sites/joomla # сделать чрез переменую?  {{ path_to_joomla_site_folder }}

unzip joomla.zip -d /var/www/sites/joomla

chown -R www-data: /var/www/sites/joomla # сделать чрез переменую? {{ nginx_user }}

#
#nano /etc/nginx/nginx.conf
#user  www-data;     <---

#
nano /etc/nginx/sites-available/joomla.conf

server {
        listen 80;
        server_name {{ joomla_ip }};
        root /var/www/sites/joomla;
        index index.php index.html index.htm;
        location / {
                try_files $uri $uri/ =404;
        }
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        }
}



ln -s /etc/nginx/sites-available/joomla.conf /etc/nginx/sites-enabled/joomla.conf

nginx -t

systemctl restart nginx


cat << EOF >  file.sql
CREATE DATABASE joomla_db;
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'LNGsgS4rxC7t7KmLaP9q';
GRANT ALL PRIVILEGES ON joomla_db.* TO 'admin'@'localhost';
FLUSH PRIVILEGES;
EOF

mysql < file.sql

mysql -e 'SELECT User FROM mysql.user;'
mysql -e 'show databases;



go to http://192.168.0.77

```
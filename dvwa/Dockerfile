FROM php:7.4-apache

RUN apt update
RUN apt install -y libpng-dev
RUN docker-php-ext-install gd pdo pdo_mysql mysqli
RUN a2enmod rewrite


RUN git clone https://github.com/digininja/DVWA.git /var/www/html/


RUN sed -i "s/allow_url_include = Off/allow_url_include = On/" /usr/local/etc/php/php.ini-production
RUN sed -i "s/'dvwa'/'root'/" /var/www/html/config/config.inc.php.dist
RUN sed -i "s/p@ssw0rd/anbom123/" /var/www/html/config/config.inc.php.dist
RUN sed -i "s/'127.0.0.1'/'192.168.43.133'/" /var/www/html/config/config.inc.php.dist
RUN mv /var/www/html/config/config.inc.php.dist  /var/www/html/config/config.inc.php

RUN chown -R www-data:www-data /var/www/html/

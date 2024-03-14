# Sử dụng image php apache làm base
FROM php:7.4-apache

# Cài đặt các phụ thuộc
RUN apt-get update && apt-get install -y \
    git \
    libpng-dev \
    && docker-php-ext-install gd pdo pdo_mysql mysqli \
    && a2enmod rewrite

# Clone DVWA vào thư mục root của web server
RUN git clone https://github.com/digininja/DVWA.git /var/www/html/

# Cấu hình DVWA
RUN sed -i "s/allow_url_include = Off/allow_url_include = On/" /usr/local/etc/php/php.ini-production
RUN sed -i "s/p@ssw0rd/anbom123/" /var/www/html/config/config.inc.php.dist
RUN sed -i "s/'127.0.0.1'/'10.1.36.38'/" /var/www/html/config/config.inc.php.dist
RUN sed -i "s/'dvwa'/'root'/" /var/www/html/config/config.inc.php.dist
RUN mv /var/www/html/config/config.inc.php.dist  /var/www/html/config/config.inc.php

RUN chown -R www-data:www-data /var/www/html/



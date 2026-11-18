FROM php:8.5.12-apache

# ติดตั้งแพ็กเกจและ Extensions พื้นฐานที่เทียบเคียงกับระบบเดิม (cURL, GD, MySQLi, PDO, Zip ฯลฯ)
RUN apt-get update && apt-get install -y \
    build-essential \
    procps \
    file \
    git \
    openssl \
    libnss3-tools \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libzip-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    zip \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql mysqli curl zip

# ตั้งค่าโฟลเดอร์และสร้าง Self-signed Certificate สำหรับ SSL
RUN mkdir -p /etc/apache2/ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=TH/ST=Lampang/L=Muang/O=Surasak/CN=localhost" -addext "subjectAltName=IP:192.168.129.89,IP:127.0.0.1,DNS:localhost"

# เปิดใช้งานโมดูล Apache SSL และ Rewrite พร้อมเปิดใช้งานเว็บไซต์ SSL
RUN a2enmod ssl && a2enmod rewrite

ENV APACHE_DOCUMENT_ROOT /var/www/html
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# คัดลอกไฟล์คอนฟิก VirtualHost ทั้ง HTTP (พอร์ต 80) และ HTTPS (พอร์ต 443)
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ./default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

# เปิดใช้งานเว็บไซต์รองรับ SSL
RUN a2ensite default-ssl

COPY ./index.php /var/www/html/index.php

EXPOSE 80
EXPOSE 443

CMD ["apache2-foreground"]
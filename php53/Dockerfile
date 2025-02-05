FROM httpd:2.2
COPY ./sources.list /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y --force-yes software-properties-common
RUN apt-get install -y --force-yes debian-archive-keyring build-essential gcc make re2c vim procps file git 
RUN apt-get install -y --force-yes mysql-client imagemagick libmysqlclient-dev libxml2-dev libpng-dev libjpeg-dev libxv-dev curl libcurl4-openssl-dev libcurl3 libcurl3-dev libcurl3-gnutls zlib1g zlib1g-dev libxslt1-dev libzip-dev libxml2 libsnmp-base libxml2-dev libsnmp-dev libjpeg62 libpng12-0 libpng12-dev zlib1g zlib1g-dev libfreetype6 libfreetype6-dev libbz2-dev libmcrypt-dev libmcrypt4
RUN apt-get install -y --force-yes php5-dev php5-curl php5-imagick
COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

RUN curl -o php-5.3.29.tar.gz https://museum.php.net/php5/php-5.3.29.tar.gz
RUN tar -zxvf php-5.3.29.tar.gz

RUN cd php-5.3.29 && ./configure --with-apxs2=/usr/local/apache2/bin/apxs --enable-mbstring --enable-zip --enable-exif --with-curl=/usr/bin/curl --with-zlib --with-mysql --with-mysqli=/usr/bin/mysql_config --with-pdo-mysql --with-xsl --with-xmlrpc --with-snmp --with-bz2 --with-mcrypt --with-gd --with-jpeg-dir --with-png-dir && make && make install

COPY ./php.ini /usr/local/apache2/php-5.3.29/php.ini

COPY ./index.html /usr/local/apache2/htdocs/index.html
COPY ./index.php /usr/local/apache2/htdocs/index.php

EXPOSE 80
CMD ["httpd-foreground"]
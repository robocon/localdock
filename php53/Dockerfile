FROM httpd:2.2
COPY ./sources.list /etc/apt/sources.list
RUN apt-get update 
RUN apt-get install debian-archive-keyring
RUN apt-get update && apt-get install -y --force-yes debian-archive-keyring mysql-client libmysqlclient-dev gcc libxml2-dev make curl vim

COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

RUN curl -o php-5.3.29.tar.gz https://museum.php.net/php5/php-5.3.29.tar.gz
RUN tar -zxvf php-5.3.29.tar.gz
RUN cd php-5.3.29 && ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql --with-mysqli=/usr/bin/mysql_config && make && make install

COPY ./php.ini /usr/local/apache2/php-5.3.29/php.ini

COPY ./index.html /usr/local/apache2/htdocs/index.html
COPY ./index.php /usr/local/apache2/htdocs/index.php

EXPOSE 80
CMD ["httpd-foreground"]
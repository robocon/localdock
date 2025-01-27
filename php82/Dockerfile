FROM php:8.2-apache
RUN apt-get update && apt-get install -y build-essential procps curl file git openssl libnss3-tools
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN mkdir -p /etc/apache2/ssl

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=TH/ST=Lampang/L=Muang/O=Surasak/CN=localhost"

COPY ./default-ssl.conf /etc/apache2/sites-available/default-ssl.conf
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod ssl && a2enmod rewrite
RUN a2ensite default-ssl
RUN service apache2 restart

COPY ./index.php /var/www/html/index.php

EXPOSE 80
EXPOSE 443

CMD ["apache2-foreground"]

# https://medium.com/@tshenolomos/secure-apache-with-ssl-in-docker-9efd86329129
# https://www.arubacloud.com/tutorial/how-to-enable-https-protocol-with-apache-2-on-ubuntu-20-04.aspx
# https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-22-04
# https://serverfault.com/questions/705197/http-over-port-443-vs-https-over-port-80
# https://manpages.ubuntu.com/manpages/focal/man8/a2ensite.8.html
# https://stackoverflow.com/questions/20845056/how-can-i-expose-more-than-1-port-with-docker
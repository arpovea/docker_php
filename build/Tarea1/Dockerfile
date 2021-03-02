FROM debian:latest
RUN apt update && \
    apt -y upgrade && \
    apt -y install apache2 \
    php7.3 php7.3-mysql libapache2-mod-php7.3
COPY --chown=www-data:www-data bookmedik/ /var/www/bookmedik
COPY Database.php /var/www/bookmedik/core/controller/Database.php
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod php7.3
CMD apache2ctl -D FOREGROUND
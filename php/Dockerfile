FROM php:8.1.3RC1-fpm-alpine3.14

RUN apk update && apk add \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        bash \
        nano \
    && docker-php-ext-install bcmath pdo pdo_mysql \
    && docker-php-ext-enable opcache \
    && docker-php-source delete

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm composer-setup.php

WORKDIR /usr/local/etc/php/conf.d/
COPY /php/ini/php.ini php.ini
COPY /php/ini/browscap.ini extra/browscap.ini

WORKDIR /app
COPY /app .

RUN composer install

RUN echo "alias dbR='php bin/console d:d:d --force && php bin/console d:d:c && php bin/console d:s:u --force && php bin/console app:setup-app M. admin admin admin@linkweb.fr 1995-02-11 +33 123456789 password --go'" >> ~/.bashrc

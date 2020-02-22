FROM php:7.2-apache

MAINTAINER Gulshan Maurya <gulshan.4dream@gmail.com>

ARG apache_user
ARG USER_ID
ENV APACHE_RUN_USER=$apache_user

RUN adduser --uid $USER_ID --gecos 'My Apache User' --disabled-password $APACHE_RUN_USER \
             && chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" /var/lock/apache2 /var/run/apache2 /var/www/html

RUN apt-get update && apt-get install -y \
  cron \
  gzip \
  libbz2-dev \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libsodium-dev \
  libssh2-1-dev \
  libxslt1-dev \
  lsof \
  vim \
  zip

RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN apt-get install -y libzip-dev

RUN docker-php-ext-install \
  bcmath \
  bz2 \
  calendar \
  exif \
  gd \
  gettext \
  intl \
  mbstring \
  mysqli \
  opcache \
  pcntl \
  pdo_mysql \
  soap \
  sockets \
  sysvmsg \
  sysvsem \
  sysvshm \
  xsl \
  zip

RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

RUN a2enmod rewrite

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
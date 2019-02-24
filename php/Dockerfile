FROM php:7.3-fpm

USER root

# because PHP_VERSION is set by parent image
ARG PHP_VER=7.3
ENV PHP_VER "$PHP_VER"

RUN apt-get update && apt-get install -y \
		software-properties-common \
		curl \
        git \
        sudo \
		bash-completion \
        libxml2-dev \
        libpq-dev \
        libmemcached-dev \
		libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libzip-dev \
		libcurl4-gnutls-dev \
        vim \
        nano \
        dnsutils \
        net-tools \
        netcat \
        supervisor \
        zip \
        unzip \
        mc \
        wget


RUN docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) soap \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) curl \
    && docker-php-ext-install -j$(nproc) xml \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) intl

RUN pecl install -o -f redis \
	&& docker-php-ext-enable redis.so

#	xdebug for php 7.3 is beta
RUN apt-get update && apt-get install -y wget git unzip \
    && pecl install xdebug-2.7.0RC2 \
    && docker-php-ext-enable xdebug

#	Find how to install
#	&& pecl install -o -f memcache \
#	&& docker-php-ext-enable memcache.so \
#	try https://stackoverflow.com/questions/40894385/how-can-i-install-the-php-memcached-extension-on-dockers-php7-alpine-image
#
#	and php${PHP_VER}-dev

# Меняем часовой пояс
RUN sudo ln -fs /usr/share/zoneinfo/Europe/Moscow /etc/localtime

# устанавливаем композер и плагин для асинхронной загрузки пакетов
RUN curl -L --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && /usr/local/bin/composer global require hirak/prestissimo


# копируем конфиги
COPY php-${PHP_VER}.ini         /etc/php/${PHP_VER}/fpm/php.ini
COPY conf.d/                        /etc/php/${PHP_VER}/fpm/conf.d/
COPY php-fpm.d/                     /etc/php/${PHP_VER}/fpm/pool.d/
COPY php-fpm-${PHP_VER}.conf    /etc/php/${PHP_VER}/fpm/php-fpm.conf
COPY php-${PHP_VER}.ini         /etc/php/${PHP_VER}/cli/php.ini
COPY conf.d/                        /etc/php/${PHP_VER}/cli/conf.d/
#   Not yet
#COPY .bashrc .inputrc .nanorc       /etc/skel/
#COPY .bashrc .inputrc .nanorc       /root/

WORKDIR /srv/www

# создаём директорию для сессий php
RUN mkdir -p /sessions \
  && mkdir -p /srv/www/.composer /srv/www/common

EXPOSE 9000
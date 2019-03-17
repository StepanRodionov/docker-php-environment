FROM php:7.3-fpm-alpine

USER root

# because PHP_VERSION is set by parent image
ARG PHP_VER=7.3
ENV PHP_VER "$PHP_VER"

# Missing:
# software-properties-common
# dnsutils
# libpq-dev
# netcat
# libcurl4-gnutls-dev


# https://github.com/docker-library/php/issues/225
# libjpeg62-turbo-dev - WO version
# libfreetype6-dev - WO version

# https://stackoverflow.com/questions/36530620/error-unsatisfiable-constraints-on-php7-fpm-alpine
RUN apk update \
    apk add --update \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/community \
    apk upgrade --force


RUN apk add --no-cache \
        alpine-sdk \
        curl \
        curl-dev \
        git \
        sudo \
        bash-completion \
        libxml2-dev \
        libmemcached-dev \
        libpng-dev \
        libzip-dev \
        g++ \
        icu-dev \
        vim \
        nano \
        net-tools \
        supervisor \
        zip \
        unzip \
        mc \
        freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
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

####################################################
#       WORKS BEFORE HERE


# https://stackoverflow.com/a/45976380/11217535
RUN apk add --no-cache --update --virtual buildDeps autoconf \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && pecl install -o -f redis \
    && docker-php-ext-enable redis.so \
    && pecl install apcu \
    && docker-php-ext-enable apcu.so \
    && apk del buildDeps

# Меняем часовой пояс
RUN sudo ln -fs /usr/share/zoneinfo/Europe/Moscow /etc/localtime

# устанавливаем композер и плагин для асинхронной загрузки пакетов
RUN curl -L --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && /usr/local/bin/composer global require hirak/prestissimo


# копируем конфиги
COPY configs/${PHP_VER}/php.ini            /etc/php/${PHP_VER}/fpm/php.ini
COPY configs/conf.d                        /etc/php/${PHP_VER}/fpm/conf.d/
COPY configs/php-fpm.d                     /etc/php/${PHP_VER}/fpm/pool.d/
COPY configs/${PHP_VER}/php-fpm.conf       /etc/php/${PHP_VER}/fpm/php-fpm.conf
COPY configs/${PHP_VER}/php.ini            /etc/php/${PHP_VER}/cli/php.ini
COPY configs/conf.d                        /etc/php/${PHP_VER}/cli/conf.d/
#   Not yet
#COPY .bashrc .inputrc .nanorc       /etc/skel/
#COPY .bashrc .inputrc .nanorc       /root/

WORKDIR /srv/www

# создаём директорию для сессий php
RUN mkdir -p /sessions \
  && mkdir -p /srv/www/.composer /srv/www/common

EXPOSE 9000
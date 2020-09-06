FROM php:7.4-fpm

USER root

# because PHP_VERSION is set by parent image
ARG PHP_VER=7.4
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
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
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
	&& docker-php-ext-enable redis.so \
	&& pecl install apcu \
    && pecl install xdebug \
    && docker-php-ext-enable apcu.so \
    && docker-php-ext-enable xdebug

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

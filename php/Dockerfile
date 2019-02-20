FROM php:7.3-fpm

MAINTAINER Stepan Rodionov

ARG PHP_VERSION=7.3
ENV PHP_VERSION "$PHP_VERSION"

# установка системных библиотек (обязательно должна быть выполнена в одном RUN)
RUN apt-get update \
    && apt-get -y install \
        software-properties-common \
        curl \
        git \
        sudo \
        bash-completion \
        libxml2-dev \
        libpq-dev \
        libmemcached-dev \
        vim \
        nano \
        dnsutils \
        net-tools \
        netcat \
        supervisor \
        zip \
        unzip \
        mc \
        wget \
    && apt-get clean && rm -r /var/lib/apt/lists/*

# добавление php репозитория
RUN apt-get update \
    # установка стандартных php библиотек
    && apt-get -y install   php${PHP_VERSION}-fpm \
                            php${PHP_VERSION}-mbstring \
                            php${PHP_VERSION}-opcache \
                            php${PHP_VERSION}-soap \
                            php${PHP_VERSION}-mysql \
                            php${PHP_VERSION}-zip \
                            php${PHP_VERSION}-redis \
                            php${PHP_VERSION}-curl \
                            php${PHP_VERSION}-xml \
                            php${PHP_VERSION}-bcmath \
                            php${PHP_VERSION}-intl \
                            php${PHP_VERSION}-gd \
                            php${PHP_VERSION}-dev \
                            php-xdebug \
                            php-memcache \
    && apt-get clean && rm -r /var/lib/apt/lists/*

# Меняем часовой пояс
RUN sudo ln -fs /usr/share/zoneinfo/Europe/Moscow /etc/localtime

# устанавливаем композер и плагин для асинхронной загрузки пакетов
RUN curl -L --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && /usr/local/bin/composer global require hirak/prestissimo


WORKDIR /srv/www

EXPOSE 9000
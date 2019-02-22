FROM php:7.3-fpm

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
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) dev

RUN pecl install -o -f redis \
	&& docker-php-ext-enable -j$(nproc) redis

# Меняем часовой пояс
RUN sudo ln -fs /usr/share/zoneinfo/Europe/Moscow /etc/localtime

# устанавливаем композер и плагин для асинхронной загрузки пакетов
RUN curl -L --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && /usr/local/bin/composer global require hirak/prestissimo


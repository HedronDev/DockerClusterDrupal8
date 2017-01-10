FROM php:fpm
MAINTAINER Kurt Vanderwater <kurt@meridian-ds.com>

# https://github.com/docker-library/docs/tree/master/php
#

# Set some "INSTALLATION" variables
ARG TERM=ansi
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libxml2-dev \
        ssmtp \
        dh-make-php \
    && pecl install -o -f xdebug \
    && rm -fR /tmp/pear \
    # Install xdebug
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=on"  > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=$(/sbin/ip route|awk '/default/ { print $3 }')" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_log=/tmp/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
#    xdebug.remote_host=172.18.0.2
#    && echo "memory_limit = 64M" > /usr/local/etc/php/conf.d/php.ini \
    # Install opcache
    && { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=60'; \
    echo 'opcache.fast_shutdown=1'; \
    echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini \
    # Install unconfigured php extentions
    && docker-php-ext-install -j$(nproc) \
        iconv \
        mcrypt \
        mbstring \
        opcache \
#        mysql \
        mysqli \
        pdo_mysql \
        hash \
        zip \
        xml \
        json \
    # Configure and install gd as php extension
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd


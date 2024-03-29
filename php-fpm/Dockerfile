ARG PHP_VERSION=7.2
FROM php:${PHP_VERSION}-fpm-alpine3.9

USER root

# CHANGE SOURCE && apk upgrade
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
    && apk update \
    && apk upgrade \
    && apk add --no-cache openssl \
    && apk add --no-cache bash

# Install the PHP gd zip pdo_mysql extention
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd \
    && docker-php-ext-install pdo_mysql zip \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

COPY php-fpm.conf /usr/local/etc/php-fpm.conf

CMD ["php-fpm"]

EXPOSE 9000
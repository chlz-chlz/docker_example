#syntax=docker/dockerfile:1.4
FROM php:8.3.3-fpm-alpine3.19

RUN set -eux && \
    apk update && \
    apk add --no-cache \
        libzip-dev \
        libpng-dev \
        oniguruma-dev \
        curl-dev && \
    apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS && \
    apk del && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir /root/history && \
    echo 'alias sf="php bin/console"' >> ~/.bashrc

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions \
        bcmath \
        mbstring \
        curl \
        zip \
        gd \
        @composer \
        opcache \
        pdo_mysql \
        mysqli \
        intl \
        pcov \
        pcntl && \
    ln -s composer /usr/local/bin/c

COPY --link provisioning/php/php.ini $PHP_INI_DIR/conf.d/
COPY --link provisioning/php/fpm.conf /usr/local/etc/php-fpm.d/zzz-docker.conf

RUN addgroup -g 1000 app && adduser -u 1000 -G app -s /bin/sh -D app

WORKDIR /srv/app
USER app

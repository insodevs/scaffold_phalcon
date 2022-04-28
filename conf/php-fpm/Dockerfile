FROM php:8.0.14-fpm

WORKDIR "/app"

ARG PHP_VERSION=8.0
ARG PHALCON_VERSION=5.0.0beta3
ARG PHALCON_EXT_PATH=phalcon

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN apt-get update; \
    apt-get -y --no-install-recommends install \
        php${PHP_VERSION}-bcmath \
        php${PHP_VERSION}-gd \
        php${PHP_VERSION}-gearman \
        php${PHP_VERSION}-igbinary \
        php${PHP_VERSION}-imagick \
        php${PHP_VERSION}-mcrypt \
        php${PHP_VERSION}-memcached \
        php${PHP_VERSION}-mysql \
        php${PHP_VERSION}-phpdbg \
        php${PHP_VERSION}-psr \
        php${PHP_VERSION}-redis \
        php${PHP_VERSION}-xdebug \
        php${PHP_VERSION}-yaml \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*


RUN apt-get update && apt-get install -y \
    && chmod +x /usr/local/bin/install-php-extensions \
    && install-php-extensions \
            openssl \
            mbstring \
            mcrypt \
            pdo_sqlsrv \
            pdo_pgsql \
            pdo_mysql \
            mongodb


# Phalcon
RUN mkdir  /usr/src/php/ \
        && mkdir  /usr/src/php/ext \
        && curl -L -o /tmp/psr-php.tar.gz "https://github.com/jbboehr/php-psr/archive/refs/tags/v1.0.1.tar.gz" \
        && mkdir /tmp/psr-php \
        && tar xfz /tmp/psr-php.tar.gz --strip 1 -C /tmp/psr-php \
        && rm -r /tmp/psr-php.tar.gz \
        && mv /tmp/psr-php/ /usr/src/php/ext/pasr-php \
        && rm -rf /tmp/psr-php \
        && curl -L -o /tmp/phalcon5.tar.gz "https://github.com/phalcon/cphalcon/archive/refs/tags/v5.0.0beta3.tar.gz" \
        && mkdir /tmp/phalcon5 \
        && tar xfz /tmp/phalcon5.tar.gz --strip 1 -C /tmp/phalcon5 \
        && rm -r /tmp/phalcon5.tar.gz \
        && mv /tmp/phalcon5/ext /usr/src/php/ext/phalcon5 \
        && rm -rf /tmp/phalcon5
RUN docker-php-ext-install pasr-php
RUN docker-php-ext-install phalcon5
RUN mv /usr/local/etc/php/conf.d/docker-php-ext-phalcon.ini /usr/local/etc/php/conf.d/docker-php-ext-xphalcon.ini


FROM php:8-fpm as base

ENV USER_ID=1000
ENV GROUP_ID=1000

ADD https://github.com/just-containers/s6-overlay/releases/download/v2.2.0.1/s6-overlay-amd64-installer /tmp/

RUN apt-get update \
    # install S6-Overlay
    && chmod +x /tmp/s6-overlay-amd64-installer && /tmp/s6-overlay-amd64-installer / \
    # install nginx
    && apt-get install -y --no-install-recommends nginx \
    && rm -rf /etc/nginx/sites-enabled/* \
    && mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.conf \
    && ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf \
    && mkdir -p /var/www/html \
    && mkdir -p /var/run/php \
    && mv /var/www/html/index.nginx-debian.html /var/www/html/index.html \
    && mkdir -p /var/www/public \
    && echo "<?php phpinfo(); " >> /var/www/public/index.php \
    # install cron
    && apt-get install -y --no-install-recommends cron \
    # php dev config and symfony dependencies
    && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    # essentials
    && apt-get install -y --no-install-recommends git unzip \
    # gd
    && apt-get install -y --no-install-recommends libpng-dev libjpeg-dev libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    # pdo_mysql
    && docker-php-ext-install pdo_mysql \
    # intl
    && apt-get install -y zlib1g-dev libicu-dev g++ \
    && docker-php-ext-install intl \
    # opcache
    && docker-php-ext-install opcache \
    # composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    # clear
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

COPY ./configs /

EXPOSE 80
WORKDIR /var/www

ENTRYPOINT ["/init"]

FROM base AS development

RUN apt-get update \
    # install dev tools
    && apt-get -y --no-install-recommends install vim-nox mc procps \
    # change to dev php.ini
    && cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
    # symfony
    && curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony/bin/symfony /usr/local/bin/symfony
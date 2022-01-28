# GENERATED FILE: Dockerfile for PHP images
FROM php:8.0-cli-bullseye

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

# Add docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions

# Mandatory updates and dep installs
RUN apt-get update && apt-get upgrade && apt-get install bash-completion \
    && pecl channel-update pecl.php.net

# Set TZ info
ENV TZ='Etc/UTC'
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get install -y vim zip unzip tar netcat-openbsd autoconf gcc make autoconf pkg-config g++ \
    && apt-get install -y libicu-dev libzip-dev libfreetype6-dev libcap-ng-dev \
    && apt-get install -y libpng-dev libwebp-dev libjpeg62-turbo-dev libwebp-dev zlib1g-dev libxpm-dev
# Install default PHP base extensions
RUN docker-php-ext-install exif pcntl bcmath intl zip
RUN pecl install msgpack \
    && docker-php-ext-enable --ini-name 0-msgpack.ini msgpack
RUN pecl install igbinary \
    && docker-php-ext-enable --ini-name 0-igbinary.ini igbinary
RUN install-php-extensions gd-stable

# Install deployment tools (node/composer/yarn/etc)
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs && corepack enable


# Ensure all fpm base images install, but not configure opcache...
RUN docker-php-ext-install opcache \
    && mkdir /usr/local/etc/php/off \
    && mv /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini /usr/local/etc/php/off/



RUN sed -i "s#;error_log = syslog#error_log = /dev/stderr#g" /usr/local/etc/php/php.ini-development
RUN sed -i "s#;error_log = syslog#error_log = /dev/stderr#g" /usr/local/etc/php/php.ini-production
# GENERATED FILE: Dockerfile for PHP images
FROM php:8.0-cli-alpine

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

# Add docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions

# Mandatory updates and dep installs
RUN apk update && apk upgrade \
    && pecl channel-update pecl.php.net

# Add bash and basic setup tools
RUN apk add bash bash-completion alpine-conf

# Set TZ info
ENV TZ='UTC'
RUN setup-timezone -z $TZ

RUN apk add vim zip unzip tar netcat-openbsd autoconf gcc make autoconf pkgconfig g++ \
    && apk add icu-libs icu-dev libzip libzip-dev freetype-dev libcap \
    && apk add libpng libpng-dev libwebp libwebp-dev libjpeg-turbo-dev libwebp-dev zlib-dev libxpm-dev
# Install default PHP base extensions
RUN docker-php-ext-install exif pcntl bcmath intl zip
RUN pecl install msgpack \
    && docker-php-ext-enable --ini-name 0-msgpack.ini msgpack
RUN pecl install igbinary \
    && docker-php-ext-enable --ini-name 0-igbinary.ini igbinary
RUN install-php-extensions gd-stable

# Install deployment tools (node/composer/yarn/etc)
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && apk add nodejs yarn


# Ensure all fpm base images install, but not configure opcache...
RUN docker-php-ext-install opcache \
    && mkdir /usr/local/etc/php/off \
    && mv /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini /usr/local/etc/php/off/



RUN sed -i "s#;error_log = syslog#error_log = /dev/stderr#g" /usr/local/etc/php/php.ini-development
RUN sed -i "s#;error_log = syslog#error_log = /dev/stderr#g" /usr/local/etc/php/php.ini-production
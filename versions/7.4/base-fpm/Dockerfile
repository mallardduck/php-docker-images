# GENERATED FILE: Dockerfile for PHP images
FROM php:7.4-fpm-alpine3.14

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

# Add bash and basic setup tools
RUN apk add bash bash-completion alpine-conf

# Set TZ info
ENV TZ='UTC'
RUN setup-timezone -z $TZ

# Mandatory updates and dep installs
RUN apk update && apk upgrade \
    && pecl channel-update pecl.php.net
RUN apk add vim zip unzip tar netcat-openbsd autoconf gcc make autoconf pkgconfig g++ \
    && apk add icu-libs icu-dev libzip libzip-dev freetype-dev libcap \
    && apk add libpng libpng-dev libwebp libwebp-dev libjpeg-turbo-dev libwebp-dev zlib-dev libxpm-dev

# Install default PHP base extensions
RUN docker-php-ext-install exif pcntl bcmath intl zip
RUN pecl install msgpack \
    && docker-php-ext-enable --ini-name 0-msgpack.ini msgpack
RUN pecl install igbinary \
    && docker-php-ext-enable --ini-name 0-igbinary.ini igbinary
RUN docker-php-ext-configure gd \
        --prefix=/usr --with-jpeg \
        --with-webp --with-freetype \
    && docker-php-ext-install gd

# Install deployment tools (node/composer/yarn/etc)
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && apk add nodejs yarn

# Ensure all fpm base images install, but not configure opcache...
RUN docker-php-ext-install opcache \
    && mkdir /usr/local/etc/php/off \
    && mv /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini /usr/local/etc/php/off/

# Clean up some build only things...
RUN apk del autoconf gcc make autoconf pkgconfig g++
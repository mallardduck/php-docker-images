# GENERATED FILE: Dockerfile for PHP images
FROM mallardduck/base-php:8.1-fpm

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

RUN apk add autoconf gcc make autoconf pkgconfig g++

###########################################################################
# PHP REDIS EXTENSION
###########################################################################
RUN ( \
    pecl install --nobuild redis && \
    cd "$(pecl config-get temp_dir)/redis" && \
    phpize && \
    ./configure --enable-redis-igbinary \
        --enable-redis-msgpack \
        --enable-redis-json && \
    make -j$(nproc) && \
    make install \
)

###########################################################################
# PHP Memcached:
###########################################################################
RUN apk add libmemcached libmemcached-dev
RUN ( \
    pecl install --nobuild memcached && \
    cd "$(pecl config-get temp_dir)/memcached" && \
    phpize && \
    ./configure --enable-memcached-igbinary \
        --enable-memcached-msgpack \
        --enable-memcached-json && \
    make -j$(nproc) && \
    make install \
)

RUN docker-php-ext-enable --ini-name z-cache.ini redis memcached

# Clean up some build only things...
RUN apk del autoconf gcc make autoconf pkgconfig g++
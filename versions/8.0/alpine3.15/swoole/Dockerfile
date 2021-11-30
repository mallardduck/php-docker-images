# GENERATED FILE: Dockerfile for PHP images
FROM mallardduck/base-php:8.0-mysql

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

RUN apk add autoconf gcc make autoconf pkgconfig g++ openssl-dev c-ares-dev curl-dev

###########################################################################
# PHP Swoole:
###########################################################################
RUN ( \
pecl install --nobuild openswoole && \
cd "$(pecl config-get temp_dir)/openswoole" && \
phpize && \
./configure --enable-openssl \
    --enable-mysqlnd \
    --enable-http2 \
    --enable-swoole-curl \
    --enable-swoole-json \
    --enable-cares && \
make -j$(nproc) && \
make install \
)
RUN docker-php-ext-enable --ini-name z-swoole.ini openswoole

# Clean up some build only things...
RUN apk del autoconf gcc make autoconf pkgconfig g++
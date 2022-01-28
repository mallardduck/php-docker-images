# GENERATED FILE: Dockerfile for PHP images
FROM mallardduck/base-php:8.0-fpm-bullseye

MAINTAINER Dan Pock <self@danpock.me>
LABEL maintainer="Dan Pock"

###########################################################################
# PHP REDIS EXTENSION
###########################################################################
RUN install-php-extensions redis-stable

###########################################################################
# PHP Memcached:
###########################################################################
RUN install-php-extensions memcached-stable
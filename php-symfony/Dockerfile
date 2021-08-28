FROM composer:2.1 as php-symfony-vendor

COPY composer.json composer.json
COPY composer.lock composer.lock
RUN composer install \
    --ignore-platform-reqs \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --prefer-dist

FROM webdevops/php-nginx:8.0-alpine as php-symfony

# PHP and Nginx Settings
ENV WEB_DOCUMENT_ROOT /app/public
ENV FPM_PM_START_SERVERS 10
ENV FPM_PM_MAX_CHILDREN 50
ENV FPM_PM_MIN_SPARE_SERVERS 10
ENV FPM_PM_MAX_SPARE_SERVERS 50
ENV FPM_MAX_REQUESTS 500
ENV PHP_DISPLAY_ERRORS 0

WORKDIR /app

COPY / /app

RUN chmod 777 -R /tmp
RUN chown application:application -R /tmp

VOLUME /app

FROM php-symfony

WORKDIR /app


COPY / /app/
COPY --from=php-symfony-vendor /app/vendor /app/vendor/

VOLUME /app

# avoid kill nginx during a request
STOPSIGNAL USR2
FROM php:8.1-apache

ARG PHP_XDEBUG
ARG PHP_XDEBUG_MODE='debug'
ARG APP_ENV
ENV PHP_IDE_CONFIG="serverName=convoy"

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions pdo_mysql pcntl redis ioncube gd zip

RUN if [ $PHP_XDEBUG = "true" ]; then \
            install-php-extensions xdebug; \
            echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
            echo "xdebug.mode=$PHP_XDEBUG_MODE" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
            echo "xdebug.idekey=convoy" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
            #echo "xdebug.log=/var/www/storage/logs/xdebug.log" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
            echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
        fi;

COPY ./whmcs.conf /etc/apache2/sites-available/whmcs.conf

RUN a2ensite whmcs

WORKDIR /var/www/html
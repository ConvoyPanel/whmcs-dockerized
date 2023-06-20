FROM php:8.1-fpm-alpine

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

RUN echo 'pm.max_children = 100' >> /usr/local/etc/php-fpm.d/zz-docker.conf

WORKDIR /var/www

CMD ["php-fpm"]
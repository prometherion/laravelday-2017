FROM prometherion/php7-nginx:1.1.3

#
# Installing PHP needed extensions
#
USER root
RUN apt-get update && apt-get install -y \
        sqlite3 \
        libsqlite3-dev \
    && docker-php-ext-install \
        bcmath \
        pdo_mysql \
        pdo_sqlite \
    && pecl install -o -f redis \
        &&  rm -rf /tmp/pear \
        &&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
    && composer global require hirak/prestissimo

#
# Laravel deployment
#
COPY . ./

RUN rm -rf \
    ./storage/app/public/* \
    ./storage/framework/cache/* \
    ./storage/framework/sessions/* \
    ./storage/framework/views/* \
    ./storage/logs/* \
    && touch ./storage/logs/laravel.log \
    && sed -i -r 's/;clear_env = no/clear_env = no/' $PHP_INI_DIR/../php-fpm.d/www.conf \
    && composer install \
    && chown -R www-data . \
    && rm -rf /etc/supervisor/conf.d \
        && ln -s $PWD/storage/worker /etc/supervisor \
        && mv /etc/supervisor/worker /etc/supervisor/conf.d

#
# Using user www-data instead of root
#
USER www-data

FROM php:5.6-fpm

ADD php.ini    /usr/local/etc/php/php.ini
#ADD php-fpm.conf    /usr/local/etc/php-fpm.conf

#change to 163 source
ADD sources.list /etc/apt/sources.list

COPY redis.tgz /home/redis.tgz
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        git \
    && docker-php-ext-install iconv mcrypt zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mbstring \
    && pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer

#composer mirror
RUN composer config -g repo.packagist composer http://packagist.phpcomposer.com

WORKDIR /opt
RUN usermod -u 1000 www-data

VOLUME ["/opt"]

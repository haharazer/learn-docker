FROM php:5.6-cli

#change to 163 source
ADD sources.list /etc/apt/sources.list

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
    && docker-php-ext-install mbstring 

ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer
#composer mirror
RUN composer config -g repo.packagist composer http://packagist.phpcomposer.com

#build laravel
RUN composer create-project laravel/laravel /var/php/laravel 5.1

#wordir
WORKDIR /var/php/laravel

#checkout code
RUN mkdir -p /tmp && cd /tmp \
    && git clone https://github.com/haharazer/layifa.git \
    && cp -r ./layifa/youhui/* /var/php/laravel

#copy conf
COPY env /var/php/laravel/.env

#install depency
RUN composer update
RUN chmod -R 777 /var/php/laravel/storage
RUN php artisan cache:clear 
RUN composer dump-autoload

VOLUME ["/var/php/laravel"]
CMD ["php", "artisan", "migrate"]

FROM php:7.3-fpm

#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
#

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN apt-get update && \
    apt-get install -y --force-yes --no-install-recommends \
        libzip-dev \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        openssh-server \
        swig \
        cmake \
        wget \
        git \
        cron \
        nano \
        mc \
        supervisor \
        ffmpeg \
        iproute2

#####################################
# GD:
#####################################

# Install the PHP gd library
RUN docker-php-ext-install gd && \
    docker-php-ext-configure gd \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick \
	&& docker-php-ext-enable imagick

RUN docker-php-ext-install pcntl

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql

RUN docker-php-ext-install pdo pdo_pgsql

RUN docker-php-ext-install zip

RUN docker-php-ext-install exif && \
    docker-php-ext-enable exif

#####################################
# Final Touch:
#####################################

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer global require hirak/prestissimo

#####################################
#Install xdebug:
#####################################

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp " >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=docker" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=192.168.220.1" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

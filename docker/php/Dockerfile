FROM php:7.2-fpm

LABEL maintainer="mitirrli"

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        libpng-dev \
        libjpeg-dev \
    && rm -rf /var/lib/apt/lists/* \

    # composer
    && curl -sS https://getcomposer.org/installer \
            | php -- --install-dir=/usr/local/bin --filename=composer \
    # 国内镜像加速
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
#    && pecl install xdebug \
#    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini.off

# yaconf
RUN pecl install yaconf && docker-php-ext-enable yaconf && \
    { \
        echo "yaconf.directory=/usr/local/etc/yaconf"; \
        echo "yaconf.check_delay = 10"; \
    } > /usr/local/etc/php/conf.d/yaconf.ini

# sql
RUN docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql

RUN docker-php-ext-configure gd \
      --enable-gd-native-ttf \
      --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd \
        zip \
        opcache

# opcache
RUN { \
        echo 'opcache.memory_consumption=32'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=3000'; \
        echo 'opcache.revalidate_freq=1'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# set upload size to 10M
RUN { \
        echo 'upload_max_filesize = 10M'; \
        echo 'post_max_size = 10M'; \
    } > /usr/local/etc/php/conf.d/upload-size.ini

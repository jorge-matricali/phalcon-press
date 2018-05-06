FROM php:7.1-fpm

MAINTAINER Jorge Matricali <jorgematricali@gmail.com>

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    unzip \
    curl \
    libicu-dev \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libxml2-dev \
    libbz2-dev \
    libjpeg62-turbo-dev

RUN docker-php-ext-configure bcmath --enable-bcmath && \
    docker-php-ext-configure pcntl --enable-pcntl && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql && \
    docker-php-ext-configure pdo_pgsql --with-pgsql && \
    docker-php-ext-configure mbstring --enable-mbstring && \
    docker-php-ext-configure soap --enable-soap && \
    docker-php-ext-install \
        bcmath \
        intl \
        mbstring \
        mcrypt \
        mysqli \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        soap \
        sockets \
        zip &&\
    docker-php-ext-configure gd \
      --enable-gd-native-ttf \
      --with-jpeg-dir=/usr/lib \
      --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" && \
    mkdir -p memcached && \
    tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 && \
    ( \
        cd memcached \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) && \
    rm -r memcached && \
    rm /tmp/memcached.tar.gz && \
    docker-php-ext-enable memcached

RUN curl -O https://codeload.github.com/phalcon/cphalcon/tar.gz/v3.2.0 && \
    tar xvzf v3.2.0 && \
    cd cphalcon-3.2.0/build && ./install && \
    cd ../../ && rm -Rf cphalcon-3.2.0 && rm -Rf v3.2.0 && \
    echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer

RUN curl -L 'https://phar.phpunit.de/phpunit-6.phar' > /usr/bin/phpunit && \
    chmod a+x /usr/bin/phpunit

RUN apt-get autoremove -y && \
    apt-get autoclean -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log /var/cache

FROM php:8.3.7RC1-fpm-alpine

ENV PHP_VERSION=8.3 \
    DRUSH_VERSION=12.5.1 \
    COMPOSER_VERSION=2.7.4

RUN apk add --no-cache \
    curl \
    wget \
    unzip \
    build-base \
    libxml2-dev \
    php-xml \
    libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev imap-dev \
  linux-headers oniguruma-dev libxslt-dev postgresql-dev vips-dev \
  libssh2-dev gmp-dev libzip-dev libxml2-dev freetds-dev yaml-dev \
    php-common \
    openssl-dev \
    git \
    $PHPIZE_DEPS

#RUN docker-php-ext-install dom pdo pdo_mysql opcache zip
#RUN  docker-php-ext-configure gd --with-png-dir=/usr/include --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION}
# Add Composer binary directory to PATH
ENV PATH="/usr/local/bin:${PATH}"
# Set minimum stability to dev
RUN composer global config minimum-stability dev
RUN composer global require drush/drush:${DRUSH_VERSION} --ignore-platform-req=ext-dom --prefer-dist --optimize-autoloader --update-no-dev
RUN git clone https://github.com/netdata/libjudy
RUN mkdir -p /libjudy/doc/man/man3
RUN cd /libjudy && sed -i 's/automake-1.9/automake/' bootstrap && ./bootstrap && ./configure && make install


RUN wget https://github.com/arnaud-lb/php-memory-profiler/archive/master.zip \
    && unzip master.zip \
    && cd php-memory-profiler-3 \
    && phpize \
    && ./configure --with-php-config=/usr/local/bin/php-config \
    && make \
    && make install \
    && echo "extension=memory_profiler.so" > /etc/php8/conf.d/memory_profiler.ini

COPY php-fpm.conf /etc/php8/php-fpm.conf

RUN adduser -D -u 1000 www-data

RUN mkdir -p /run/php

EXPOSE 9000

CMD ["php-fpm8", "-F"]

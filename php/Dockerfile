FROM php:8.3-fpm-alpine



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
    php-mbstring \
    php-pecl-memcached \
    php-gd \
    php-pdo \
    php-pdo_mysql \
    php-opcache \
    libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev imap-dev \
  linux-headers oniguruma-dev libxslt-dev postgresql-dev vips-dev \
  libssh2-dev gmp-dev libzip-dev libxml2-dev freetds-dev yaml-dev \
    php-common \
    openssl-dev \
    git \
    judy-dev bsd-compat-headers \
    $PHPIZE_DEPS \
    #remove default pool
#    && rm  /etc/php82/php-fpm.d/www.conf \
    && addgroup nobody tty \
    #initial php-fpm socker volume
    && mkdir /sock \
    && chown -R www-data:www-data /sock\
    && mkdir -p /var/log/php-fpm \
    && touch php-fpm.log \
    && chown -R www-data:www-data /var/log/php-fpm


RUN docker-php-ext-install gd  zip intl  mbstring dom sockets
#RUN  docker-php-ext-configure gd --with-png-dir=/usr/include --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION}
# Add Composer binary directory to PATH
ENV PATH="/usr/local/bin:${PATH}"
# Set minimum stability to dev
RUN composer global config minimum-stability dev
RUN composer global require drush/drush:${DRUSH_VERSION}  --prefer-dist --optimize-autoloader --update-no-dev



RUN wget https://github.com/arnaud-lb/php-memory-profiler/archive/master.zip \
    && unzip master.zip \
    && cd php-memory-profiler-3 \
    && phpize \
    && ./configure --with-php-config=/usr/local/bin/php-config \
    && make \
    && make install \
    && echo "extension=memprof.so" > /usr/local/etc/php/conf.d/memory_profiler.ini 



COPY ./drupal /var/www/html
COPY ./php/php-fpm.conf /usr/local/etc/php-fpm.d/mondo.conf
WORKDIR /var/www/html
RUN composer install
RUN mkdir -p /run/php
VOLUME ["/sock"]
EXPOSE 9000

CMD ["php-fpm", "-F"]

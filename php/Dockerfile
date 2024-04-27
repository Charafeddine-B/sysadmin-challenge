FROM alpine


ENV PHP_VERSION=8.3 \
    DRUSH_VERSION=12.5.1 \
    COMPOSER_VERSION=2

RUN apk --no-cache add \
    php8 \
    php8-fpm \
    php8-mbstring \
    php8-gd \
    php8-pecl-memcached \
    php8-zip \
    php8-intl \
    curl \
    wget \
    unzip \
    build-base \


RUN curl -sS https://getcomposer.org/installer | php8 -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION}


RUN composer global require drush/drush:${DRUSH_VERSION} --prefer-dist --optimize-autoloader --update-no-dev


RUN wget https://github.com/arnaud-lb/php-memory-profiler/archive/master.zip \
    && unzip master.zip \
    && cd php-memory-profiler-master \
    && phpize8 \
    && ./configure --with-php-config=/usr/bin/php-config8 \
    && make \
    && make install \
    && echo "extension=memory_profiler.so" > /etc/php8/conf.d/memory_profiler.ini


COPY php-fpm.conf /etc/php8/php-fpm.conf


RUN adduser -D -u 1000 www-data


RUN mkdir -p /run/php



EXPOSE 9000


CMD ["php-fpm8", "-F"]

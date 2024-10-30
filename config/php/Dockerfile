FROM php:8.3-fpm
RUN apt-get update \
        && apt-get install -y libcurl4-openssl-dev git pkg-config libssl-dev libmemcached-dev libmemcached11 zlib1g-dev cron libonig-dev libpq-dev libxml2-dev \
        && pecl install memcached \
        && pecl install mongodb \
        && pecl install apcu \
        && docker-php-ext-install pcntl \
        && docker-php-ext-enable memcached \
        && docker-php-ext-enable mongodb \
        && docker-php-ext-enable pcntl \
        && docker-php-ext-install mysqli pdo pdo_mysql opcache soap \
        && docker-php-ext-enable apcu \
        && docker-php-ext-enable soap \
        && apt-get clean

RUN pecl channel-update pecl.php.net

RUN apt-get update \
    && pecl install xdebug-3.3.2 \
    && docker-php-ext-enable xdebug \
    && pecl clear-cache \
    && apt-get clean

RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin --filename=composer \
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            libwebp-dev \
            libxpm-dev \
            libmcrypt-dev \
            libonig-dev; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --prefix=/usr \
            --with-jpeg \
            --with-webp \
            --with-xpm \
            --with-freetype; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'

WORKDIR /app

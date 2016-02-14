# Composer Docker Container
# Base Dockerfile: composer/base
FROM php:7-cli
MAINTAINER khanhtruong <khanhtruong@pimmor.com>

# Packages
RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng12-dev \
    libbz2-dev \
    libicu-dev \
    libssl-dev \
    php-pear \
    curl \
    git \
    subversion \
    openssh-client \
  && rm -r /var/lib/apt/lists/*

# PHP Extensions
RUN curl -L -o /tmp/igbinary.tar.gz https://github.com/igbinary/igbinary7/archive/master.tar.gz \
    && tar xfz /tmp/igbinary.tar.gz \
    && rm -r /tmp/igbinary.tar.gz \
    && ( \
        cd igbinary7-master \
        && phpize \
        && ./configure CFLAGS="-O2 -g" --enable-igbinary \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r igbinary7-master \
    && docker-php-ext-enable igbinary

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/php7.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && ( \
        cd phpredis-php7 \
        && phpize \
        && ./configure --enable-redis-igbinary \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r phpredis-php7 \
    && docker-php-ext-enable redis
RUN docker-php-ext-install -j$(nproc) mcrypt zip bz2 mbstring intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
RUN pecl install mongodb && docker-php-ext-enable mongodb

# Memory Limit
RUN echo "memory_limit=1024M" > $PHP_INI_DIR/conf.d/memory-limit.ini

# Time Zone
RUN echo "date.timezone=${PHP_TIMEZONE:-UTC}" > $PHP_INI_DIR/conf.d/date_timezone.ini

# Environmental Variables
ENV COMPOSER_HOME /root/composer

# Display PHP version
RUN php --version

# Set up the application directory
VOLUME ["/app"]
WORKDIR /app

# Set up the command arguments
CMD ["-"]
ENTRYPOINT ["composer", "--ansi"]

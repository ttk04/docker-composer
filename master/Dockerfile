# Composer Docker Container
FROM minebrand/composer:base
MAINTAINER khanhtruong <khanhtruong@pimmor.com>

ENV COMPOSER_VERSION master

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Display version information
RUN composer --version

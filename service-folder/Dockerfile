FROM php:7.2-fpm

RUN apt-get update && apt-get install -y libmcrypt-dev mysql-client
RUN pecl install mcrypt-1.0.1
RUN docker-php-ext-enable mcrypt
# Install Git and Dependencies
RUN apt-get install git -y
RUN apt-get install wget -y
RUN apt-get install zip unzip -y
# Install and enable MongoDB
RUN apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev
RUN pecl install mongodb && docker-php-ext-enable mongodb
RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini
#Install PostgreSQL
RUN apt-get install -y libpq-dev && docker-php-ext-install pdo pdo_pgsql

# Install PHPUnit
RUN curl -sSL https://phar.phpunit.de/phpunit.phar -o phpunit.phar
RUN chmod +x phpunit.phar
RUN mv phpunit.phar /usr/local/bin/phpunit 

# Install redis for php
RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install zip

# Install Composer
RUN EXPECTED_COMPOSER_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig)  \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '${EXPECTED_COMPOSER_SIGNATURE}') { echo 'Composer.phar Installer verified'; } else { echo 'Composer.phar Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

# Replace the {{SERVICE_NAME}} placeholder with the defined service from your docker-compose.yml
WORKDIR /var/www/html/{{SERVICE_NAME}}
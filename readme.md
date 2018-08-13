This is a simple set of instructions on how you can create your docker environment for your local development:
1. First remove the git link from your root directory (go to your git bash and type rm -rf .git)
2. Clone your service inside the root directory and create a Dockerfile then paste the following contents:

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

3. Remember that the folder directory of your cloned repository shall be your service name defined inside the docker-compose.yml
4. Create a vhost for your newly cloned service by creating a new vhost.conf file (name should be prefixed with the service folder name e.g. f3-api-vhost.conf) and save it in ".nginx" folder directory
5. Copy the contents of your newly created vhost config file and replace all the values of the marked placeholder (all {{SERVICE_NAME}} variables) with the appropriate service name
6. Modify the Dockerfile inside ".nginx" directory and follow the instructions stated inside (The instructions are to copy the created vhost file from your local machine to the container machine upon running)
7. Modify your docker-compose.yml and replace all marked placeholder values (all {{SERVICE_NAME}} variables) with the appropriate service name

Notes: 
 - Please make sure to replace all placeholder variables, it will also be easier to name your folder the same with the service name defined in your docker-compose.yml as a convention.
 - Inside your docker-compose.yml file, you'll see a block of code inside "environment", this is where you'll defined the values of your .env file

8. Once done with the above steps, open your terminal and go to the directory of your docker environment and run "docker-compose up --build -d"
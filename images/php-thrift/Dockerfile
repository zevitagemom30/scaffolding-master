#origin
FROM php:8.1-apache AS php8

ARG app_folder

#copies
COPY source/$app_folder/ /var/www/html

RUN chown -R www-data:www-data /var/www/html/ \
    && a2enmod rewrite

RUN service apache2 restart

#workdir
WORKDIR /var/www/html

#tools
RUN apt-get -y update --fix-missing

#npm
RUN apt-get install -y \
    npm

#node
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs

#git
RUN apt-get install -y \
    git \
    tree \
    vim \
    wget \
    subversion \
    thrift-compiler

#zip
RUN apt-get install -y \
    libzip-dev \
    zip

#postgre
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

#extensions
RUN docker-php-ext-install pdo pdo_pgsql pgsql
RUN docker-php-ext-install zip

#commands
RUN rm -rf /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

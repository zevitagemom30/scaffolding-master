#origin
FROM php:8.1-apache AS php8

#copies
COPY source /var/www/html
#COPY images/php/run.sh /tmp

#workdir
ENV APPPATH app
WORKDIR /var/www/html/$APPPATH

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
    subversion

#zip
RUN apt-get install -y \
    libzip-dev \
    zip

#postgre
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql

#extensions
RUN docker-php-ext-install pdo pdo_pgsql pgsql
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install zip

#commands
RUN rm -rf /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
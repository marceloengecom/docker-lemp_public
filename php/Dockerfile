FROM php:8.1-fpm

WORKDIR /var/www/html/
EXPOSE 80

RUN apt-get clean && apt-get update \
    &&  apt-get install -y --no-install-recommends \
        locales \
        apt-utils \
        git \
        libicu-dev \
        g++ \
        libpng-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        libxslt-dev \
        libfreetype6-dev \
        libpq-dev \
        libjpeg62-turbo-dev \
        libgmp-dev \
        libmcrypt-dev \
        unzip \
        libpq-dev \
        nodejs \
        npm \
        wget \
        zip \
        apt-transport-https \
        lsb-release \
        ca-certificates

RUN echo "pt_BR.UTF-8 UTF-8" > /etc/locale.gen  \
    &&  locale-gen

#RUN curl -sS https://getcomposer.org/installer | php -- \
#    &&  mv composer.phar /usr/local/bin/composer


# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure intl
RUN docker-php-ext-install pdo pdo_mysql pdo_pgsql opcache intl zip calendar dom mbstring gd xsl iconv mysqli soap bcmath gmp
RUN docker-php-ext-enable pdo_mysql

# Install Imagick extension
RUN apt-get update; \    
    apt-get install -y libmagickwand-dev; \
    apt-get install -y imagemagick; \
    pecl install imagick; \
    docker-php-ext-enable imagick; \
    # Success
    true

# Install Redis extension
RUN pecl install -o -f redis &&  rm -rf /tmp/pear && docker-php-ext-enable redis

# Install APCU (cache de dados na memória)
RUN pecl install apcu && docker-php-ext-enable apcu

# Remove cache
RUN rm -rf /var/cache/apk/*
# Add UID '1000' to www-data
RUN usermod -u 1000 www-data

# Permissions
#ARG WWWUSER=www-data
#ARG WWWGROUP=www-data
RUN chown -R www-data:www-data /var/www/html/
RUN find /var/www/html -type d -exec chmod 775 {} \;
RUN find /var/www/html -type f -exec chmod 664 {} \;
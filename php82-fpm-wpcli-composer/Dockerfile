FROM php:8.2.11-fpm
# Set working directory
WORKDIR /var/www/html
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
            libzip-dev \
            zlib1g-dev \
            libonig-dev; \
    rm -rf /var/lib/apt/lists/*
#configure php libraries
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
RUN docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
docker-php-ext-install gd; \
docker-php-ext-install mbstring; \
docker-php-ext-install pdo ; \
docker-php-ext-install pdo_mysql; \
docker-php-ext-install zip; \
docker-php-ext-install mysqli && docker-php-ext-enable mysqli; 


#Installing composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
	php composer-setup.php && \
	mv composer.phar /usr/local/bin/composer && \
	php -r "unlink('composer-setup.php');"

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
php wp-cli.phar --info && \
chmod +x wp-cli.phar && \
mv wp-cli.phar /usr/local/bin/wp


#Installing drush
RUN composer global require drush/drush && \
    composer global update
    
#Removing all the contents inside webroot directory
RUN rm -rf /var/www/html/*

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]

FROM php:7.2-fpm
# Set working directory
WORKDIR /var/www/html
RUN set -ex; \
	#installing OS dependencies
	apt-get update; \
	apt-get upgrade -y; \
	apt-get autoremove -y; \
	apt-get install -y \
		mariadb-client -y \
		libfreetype6-dev \
		libjpeg-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
		libmcrypt-dev \
		libmemcached-dev \
		libicu-dev \
		libpq-dev \
		curl \
		zip \
		unzip \
		git \
		nano \
		wget \
		sudo \
		vim;

#configure php libraries
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


#Removing all the contents inside webroot directory
RUN rm -rf /var/www/html/*

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]

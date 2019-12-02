# Base image
FROM php:7.3-apache

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Run apt update and install some dependancies
RUN apt update && apt install -y apt-utils sendmail mariadb-client pngquant unzip zip libpng-dev libmcrypt-dev git \
  curl libicu-dev libxml2-dev libssl-dev libcurl4 libcurl4-gnutls-dev libsqlite3-dev libsqlite3-0 libzip-dev libzip4

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

# Install phpunit
RUN curl -L -o /usr/bin/phpunit https://phar.phpunit.de/phpunit-8.phar && chmod +x /usr/bin/phpunit

# Install and enable xdbug
RUN yes | pecl install xdebug \
  && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
  && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
  && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

# Install php extensions
RUN apt-get install -y libfreetype6-dev libjpeg-dev libjpeg62-turbo libjpeg62-turbo-dev libpng-dev libpng16-16 libwebp-dev libgmp-dev libpng-dev libpng-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-webp-dir=/usr/include/  --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install mysqli bcmath intl xml curl pdo_mysql pdo_sqlite hash zip dom session opcache

# You can specify apache user there
#ENV APACHE_RUN_USER=1000

# Update public web root
ENV APACHE_DOCUMENT_ROOT /var/www/html/web
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Enable mod_rewrite
RUN a2enmod rewrite
RUN a2enmod headers
RUN a2enmod expires

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g yarn

# Install mailhog sendmail
RUN curl -fsSL "https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64" -o /usr/local/bin/mhsendmail && chmod +x /usr/local/bin/mhsendmail

FROM php:7.4-fpm
WORKDIR /var/www
RUN apt-get update && apt-get install -y \
    openssl \
    zip \
    unzip \
    git \
    build-essential \
    default-mysql-client \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libmcrypt-dev \
    libonig-dev \
    libzip-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-freetype  --with-jpeg
RUN docker-php-ext-install gd
RUN usermod -u 1000 www-data
COPY ./my-laravel-app-with-docker/ ./boot-application.sh /var/www/
RUN useradd -G www-data,root -u 1001 -d /home/gauravkumar gauravkumar
RUN mkdir -p /home/gauravkumar/.composer && \
    chown -R gauravkumar:www-data /home/gauravkumar

RUN find -type f -exec chmod 664 {} \; 
RUN find -type d -exec chmod 775 {} \; 
RUN chgrp -R www-data storage bootstrap/cache 
RUN chmod -R ug+rwx storage bootstrap/cache
RUN chown -R gauravkumar:www-data storage bootstrap/cache
RUN composer install
RUN composer dump-autoload
RUN chmod +x boot-application.sh
RUN cp .env.example .env
EXPOSE 9000
ENTRYPOINT ["./boot-application.sh"]
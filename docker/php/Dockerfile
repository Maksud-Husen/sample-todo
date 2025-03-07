FROM php:8.1-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    curl \
    libonig-dev \
    libzip-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl



COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html/Todo/

COPY . /var/www/html/Todo/

EXPOSE 9000

CMD ["php-fpm"]

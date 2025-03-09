FROM php:8.1-fpm

# Set working directory
WORKDIR /var/www/html/Todo/

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    curl \
    libonig-dev \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl gd bcmath \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy only necessary files first to leverage caching
COPY composer.json composer.lock ./

# Install dependencies
RUN composer install --no-dev --no-scripts --prefer-dist --no-progress --no-interaction




# Copy application files
COPY . /var/www/html/Todo/

RUN php artisan key:generate
RUN php artisan migrate

# Expose port for PHP-FPM
EXPOSE 9000

# Start PHP-FPM server
CMD ["php-fpm"]

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


COPY . /var/www/html/Todo/
COPY .env.example .env
RUN chmod 777 .env

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');"

# Install dependencies
# RUN composer install --no-dev --no-scripts --prefer-dist --no-progress --no-interaction
RUN sed 's_@php artisan package:discover_/bin/true_;' -i composer.json \
    && composer install --ignore-platform-req=php --no-dev --optimize-autoloader \
    && composer clear-cache \
    && php artisan package:discover --ansi \
    && chmod -R 775 storage \
    && chown -R www-data:www-data storage \
    && mkdir -p storage/framework/sessions storage/framework/views storage/framework/cache




# Copy application files


RUN php artisan key:generate

# Expose port for PHP-FPM
EXPOSE 9000

# Start PHP-FPM server
CMD ["php-fpm"]















# FROM php:8.3.11-fpm

# # Update package list and install dependencies
# RUN apt-get update && apt-get install -y \
#     libzip-dev \
#     libpng-dev \
#     postgresql-client \
#     libpq-dev \
#     nodejs \
#     npm \
#     && apt-get clean \
#     && rm -rf /var/lib/apt/lists/*

# # Install Composer
# COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# ENV COMPOSER_ALLOW_SUPERUSER=1

# # Install required packages
# RUN docker-php-ext-install pdo pgsql pdo_pgsql gd bcmath zip \
#     && pecl install redis \
#     && docker-php-ext-enable redis

# WORKDIR /var/www/html/Todo/

# # Copy the codebase
# COPY . /var/www/html/Todo/

# # Run composer install for production and give permissions
# RUN sed 's_@php artisan package:discover_/bin/true_;' -i composer.json \
#     && composer install --ignore-platform-req=php --no-dev --optimize-autoloader \
#     && composer clear-cache \
#     && php artisan package:discover --ansi \
#     && chmod -R 775 storage \
#     && chown -R www-data:www-data storage \
#     && mkdir -p storage/framework/sessions storage/framework/views storage/framework/cache

# # Copy entrypoint
# COPY ./scripts/php-fpm-entrypoint /usr/local/bin/php-entrypoint

# # Give permisisons to everything in bin/
# RUN chmod a+x /usr/local/bin/*

# ENTRYPOINT ["/usr/local/bin/php-entrypoint"]

# CMD ["php-fpm"]

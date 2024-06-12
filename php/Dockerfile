FROM php:fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql mbstring gd xml zip

# Install Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

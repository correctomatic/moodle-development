FROM php:8.0-fpm

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
    libpq-dev \
    libicu-dev \
    libxmlrpc-epi-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql mbstring gd xml zip pgsql intl soap

# Install Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Install OPcache
RUN docker-php-ext-install opcache \
    && docker-php-ext-enable opcache

# Install xmlrpc
RUN pecl install xmlrpc-1.0.0RC3 \
    && docker-php-ext-enable xmlrpc

# Set max_input_vars
RUN echo "max_input_vars = 5000" >> /usr/local/etc/php/php.ini

# Debug tools
RUN apt-get install -y procps iputils-ping \
    && echo "alias ll='ls -la'" >> /root/.bashrc

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

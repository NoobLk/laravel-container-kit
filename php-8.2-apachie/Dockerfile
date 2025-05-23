# Global Laravel Application Deployer Dockerfile For WEBLANKAN v1.0.3 by Lahiru

# Use PHP 8.2 with Apache
FROM php:8.2-apache

# Maintainer information
LABEL maintainer="Lahiru"

# Install System Dependencies
RUN apt-get update && apt-get install -y \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libssl-dev \  
    openssl \  
    zip \
    unzip \
    git \
    curl \
    nano \
    supervisor \
    redis-server

# Install Node.js 18 and npm
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g pm2

# Install PHP Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Enable Apache mod_rewrite for URL rewriting
RUN a2enmod rewrite

# Install PHP extensions required for Laravel
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql zip opcache intl bcmath mbstring exif pcntl soap ftp

# Enable FTP over SSL (FTPS) support
RUN docker-php-ext-install ftp && docker-php-ext-enable ftp

# Set PHP Configuration Options
RUN echo "expose_php = Off\n" \
    "memory_limit = 512M\n" \
    "upload_max_filesize = 50M\n" \
    "post_max_size = 50M\n" \
    "max_execution_time = 180\n" \
    "max_input_time = 300\n" \
    "max_input_vars = 10000\n" \
    "opcache.memory_consumption=128\n" \
    "opcache.interned_strings_buffer=8\n" \
    "opcache.max_accelerated_files=4000\n" \
    "opcache.revalidate_freq=2\n" \
    "opcache.fast_shutdown=1\n" \
    > /usr/local/etc/php/conf.d/php-optimizations.ini

# Configure Apache DocumentRoot to point to Laravel's public directory
ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf


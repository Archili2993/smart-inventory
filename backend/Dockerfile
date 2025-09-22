# ---------- Base Image ----------
FROM php:8.2-fpm-alpine AS base

WORKDIR /var/www/html

# System dependencies
RUN apk add --no-cache \
    bash \
    git \
    zip \
    unzip \
    unixodbc \
    unixodbc-dev \
    freetds \
    freetds-dev \
    icu-dev \
    libxml2-dev \
    oniguruma-dev \
    $PHPIZE_DEPS \
    linux-headers \
    autoconf \
    gcc \
    g++ \
    make \
    libc-dev \
    pkgconf \
    re2c

# PHP extensions
RUN docker-php-ext-install pdo mbstring exif pcntl bcmath intl

# SQLSRV extensions
RUN pecl install sqlsrv pdo_sqlsrv \
    && docker-php-ext-enable sqlsrv pdo_sqlsrv

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY . .

# ---------- Development Stage ----------
FROM base AS development

ENV ENVIRONMENT=development

# Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Install composer dev dependencies
RUN composer install

CMD ["php-fpm"]

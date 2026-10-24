FROM php:8.4-fpm

# Arguments defined in docker-compose.yml
ARG USER=sponsora
ARG UID=1000

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    libzip-dev \
    libicu-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions required by Laravel
RUN docker-php-ext-configure intl \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip intl

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install Node.js (v20 LTS) & npm for frontend builds (Vite, TailwindCSS)
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Create system user to run Composer and Artisan commands safely
RUN if ! id -u $UID > /dev/null 2>&1; then \
        useradd -G www-data,root -u $UID -d /home/$USER $USER; \
    fi \
    && mkdir -p /home/$USER/.composer \
    && chown -R $USER:$USER /home/$USER

# Set working directory
WORKDIR /var/www/html

USER $USER

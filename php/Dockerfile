FROM php:7.4.7-cli

# Fixes some weird terminal issues such as broken clear / CTRL+L
ENV TERM=linux

# Ensure apt doesn't ask questions when installing stuff
ENV DEBIAN_FRONTEND=noninteractive

# Composer Environment
ENV COMPOSER_HOME=/root/composer
ENV COMPOSER_VERSION=master
ENV COMPOSER_ALLOW_SUPERUSER=1

# Update packages
RUN apt-get update -y --fix-missing

# Install curl, git & gnupg
RUN apt-get install -y --no-install-recommends \
    apt-transport-https \
    autoconf \
    build-essential \
    ca-certificates \
    curl \
    ghostscript \
    git \
    gnupg2 \
    libbz2-dev \
    libc-client-dev \
    libcurl4-openssl-dev \
    libgmp-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libkrb5-dev \
    libpng-dev \
    libtool \
    libxml2-dev \
    libzip-dev \
    libmcrypt-dev \
    libmemcached-dev \
    libpq-dev \
    postgresql-server-dev-11 \
    software-properties-common \
    unzip \
    zlibc \
    zlib1g-dev

# Add Repositories
RUN echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C \
    && curl https://download.docker.com/linux/ubuntu/gpg -o docker_signing.key --insecure \
    && apt-key add docker_signing.key

# Install PHP extensions
RUN docker-php-ext-install \
       bcmath \
       bz2 \
       exif \
       gmp \
       opcache \
       pdo_mysql \
       pdo_pgsql \
       xmlrpc \
       zip

# Install GD
RUN apt-get update \
    && docker-php-ext-configure gd \
	--with-freetype=/usr/include/ \
	--with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && apt-get purge -y \
	libfreetype6-dev \
	libpng-dev \
	libjpeg-dev

# Install Intl
RUN docker-php-ext-install -j$(nproc) intl \
    && apt-get purge -y \
	libicu-dev

# Install IMAP
RUN docker-php-ext-configure imap \
       --with-kerberos \
       --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) imap \
    && apt-get purge -y \
	libc-client-dev \
	libkrb5-dev

# Install Memcached
RUN pecl install memcached-3.1.5 \
    && docker-php-ext-enable memcached

# Install Mcrypt
RUN pecl install mcrypt-1.0.3 \
    && docker-php-ext-enable mcrypt

# Install Redis
RUN pecl install redis-5.1.1 \
    && docker-php-ext-enable redis

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && composer clear-cache \
    && rm -rf ~/.composer

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /var/cache/apk/*

# See PHP Version
RUN php -v

# See Composer Version
RUN composer --version
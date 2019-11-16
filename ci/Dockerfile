FROM ubuntu:bionic

# Fixes some weird terminal issues such as broken clear / CTRL+L
ENV TERM=linux

# Ensure apt doesn't ask questions when installing stuff
ENV DEBIAN_FRONTEND=noninteractive

# Composer Environment
ENV COMPOSER_HOME=/root/composer
ENV COMPOSER_VERSION=master
ENV COMPOSER_ALLOW_SUPERUSER=1

# Update packages
RUN apt-get update -y

# Install curl & gnupg
RUN apt-get install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common \
    unzip

# Add Repositories
RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu bionic main" > /etc/apt/sources.list.d/ondrej-php.list \
    && echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C \
    && curl https://download.docker.com/linux/ubuntu/gpg -o docker_signing.key --insecure \
    && apt-key add docker_signing.key

# Install nginx, php, fpm & extensions
RUN apt-get update \
    && apt-get -y --no-install-recommends install \
       docker-ce \
       php-apcu \
       php-apcu-bc \
       php-imagick \
       php-memcached \
       php-yaml \
       php-zmq \
       php7.4-bcmath \
       php7.4-bz2 \
       php7.4-cli \
       php7.4-curl \
       php7.4-fpm \
       php7.4-gd \
       php7.4-gmp \
       php7.4-json \
       php7.4-mbstring \
       php7.4-mysql \
       php7.4-odbc \
       php7.4-opcache \
       php7.4-phpdbg \
       php7.4-readline \
       php7.4-xml \
       php7.4-xmlrpc \
       php7.4-zip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && composer clear-cache \
    && rm -rf ~/.composer

# Install Node
RUN curl -sL https://deb.nodesource.com/setup_13.x -o node_setup.sh \
    && bash node_setup.sh \
    && apt-get install -y nodejs build-essential

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /var/cache/apk/*

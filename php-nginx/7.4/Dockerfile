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
    && echo "deb [arch=amd64] http://nginx.org/packages/mainline/ubuntu/ bionic nginx" > /etc/apt/sources.list.d/nginx.list \
    && echo "deb-src http://nginx.org/packages/mainline/ubuntu/ bionic nginx" &>> /etc/apt/sources.list.d/nginx.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C \
    && curl -O https://nginx.org/keys/nginx_signing.key --insecure \
    && apt-key add nginx_signing.key

# Install nginx, php, fpm & extensions
RUN apt-get update \
    && apt-get -y --no-install-recommends install \
       nginx \
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

# Install supervisor
RUN apt-get update \
    && apt-get -y --no-install-recommends install \
       python-pip \
       python-setuptools \
       python-wheel \
    && pip install supervisor \
    && pip install supervisor-stdout

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && composer clear-cache \
    && rm -rf ~/.composer

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /var/cache/apk/*

# Link log files
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# Supervisor config
COPY supervisord.conf /etc/supervisord.conf

# Add nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Add php-fpm config
COPY php-fpm-www.conf /etc/php/7.4/fpm/pool.d/www.conf

# Add Scripts
COPY start.sh /start.sh

EXPOSE 80

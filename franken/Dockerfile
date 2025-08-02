FROM dunglas/frankenphp:1.9.0

# Install the packages needed to build Phalcon (no need php-dev/php-pear)
RUN apt-get update && apt-get install -y \
    git build-essential \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    jpegoptim optipng pngquant gifsicle \
    libzip-dev \
    libsqlite3-dev \
    libssl-dev \
    libcurl4-gnutls-dev \
    unixodbc-dev \
    libmcrypt-dev

RUN apt-get update && apt-get install -y \
    gnupg curl ca-certificates apt-transport-https lsb-release && \
    curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
      | gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg && \
    curl https://packages.microsoft.com/config/debian/12/prod.list \
      | tee /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql18 unixodbc-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# PHP core extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install \
        curl \
        gd \
        pdo_mysql \
        mysqli \
        pdo_sqlite \
        sockets \
        zip

#RUN echo "extension=phar" > /usr/local/etc/php/conf.d/phar.ini
# PECL extensions
RUN pecl install redis mongodb sqlsrv pdo_sqlsrv && \
    docker-php-ext-enable redis mongodb sqlsrv pdo_sqlsrv

# Phalcon PHP (từ source)
RUN git clone --depth=1 https://github.com/phalcon/cphalcon.git /tmp/cphalcon && \
    cd /tmp/cphalcon/build && ./install && \
    echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini && \
    rm -rf /tmp/cphalcon


# Composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

#RUN curl -fsSL https://get.caddyserver.com | bash -s personal

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /sbin/nologin -g www www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000

# Set timezone
ENV TZ=Asia/Ho_Chi_Minh

WORKDIR /app
COPY . /app

CMD [ "frankenphp", "run", "--config", "/etc/caddy/Caddyfile", "--watch" ]

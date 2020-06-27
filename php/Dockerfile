FROM php:7.4-fpm

# Get repository and install wget and vim
RUN apt-get update && apt-get install -y \
    wget \
    apt-utils \
    gnupg \
    software-properties-common \
    apt-transport-https \
    libxml2-dev \
    unixodbc-dev

# necessário para sqlsrv
RUN wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && wget -qO - https://packages.microsoft.com/config/debian/9/prod.list \
        > /etc/apt/sources.list.d/mssql-release.list

# Install PHP extensions deps
RUN apt-get update \
&& apt-get install --no-install-recommends -y \
libfreetype6-dev \
libjpeg62-turbo-dev \
libmcrypt-dev \
libpng-dev \
zlib1g-dev \
libicu-dev \
g++ \
unixodbc-dev \
&& ACCEPT_EULA=Y apt-get install --no-install-recommends -y msodbcsql17 mssql-tools \
&& apt-get install --no-install-recommends -y libxml2-dev \
libaio-dev \
libmemcached-dev \
freetds-dev \
libssl-dev \
openssl \
supervisor

# Install PHP extensions
RUN pecl install sqlsrv pdo_sqlsrv
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# PRA VER SE TEM ALGUM BUG NO PHP
# RUN php -i | grep "Configure Command"
RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

RUN docker-php-ext-install \
iconv \
sockets \
pdo \
pdo_mysql \
mysqli \
&& docker-php-ext-enable \
sqlsrv \
pdo_sqlsrv \
zip \
mysqli

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# Clean repository
RUN apt-get autoremove -y && \
apt-get clean && \
rm -rf /var/lib/apt/lists/*

# RUN echo "log_errors = on" \
#          > /usr/local/etc/php/php.ini-production

# RUN echo "log_errors = on" \
#          > /usr/local/etc/php/php.ini-development

# RUN echo "error_log = /var/log/php/error_log.log" \
#          > /usr/local/etc/php/php.ini-production

# RUN echo "error_log = /var/log/php/error_log.log" \
#          > /usr/local/etc/php/php.ini-development


# RUN echo "display_errors = on" \
#          > /usr/local/etc/php/php.ini-production

# RUN echo "display_errors = on" \
#          > /usr/local/etc/php/php.ini-development

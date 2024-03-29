FROM php:7.4-fpm
ARG WORKDIR_PATH
WORKDIR ${WORKDIR_PATH}
ARG USER_ID=1000
ARG GROUP_ID=1000

RUN userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi
RUN groupadd -g ${GROUP_ID} www-data &&\
    useradd -l -u ${USER_ID} -g www-data www-data;
RUN install -d -m 0755 -o www-data -g www-data /home/www-data
RUN chown -R --from=33:33 ${USER_ID}:${GROUP_ID} /home/www-data
RUN chmod -R 777 /var/www/html

# RUN apt-get update && apt-get install -y apt-transport-https

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

# Install useful tools
RUN apt-get -y install apt-utils nano wget dialog

# Install important libraries
RUN apt-get -y install --fix-missing apt-utils build-essential git curl libcurl4 libcurl3-dev zip openssl

# Composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=1.10.22


# Install xdebug
#RUN pecl install xdebug-2.6.0
#RUN docker-php-ext-enable xdebug

# Install redis
#RUN pecl install redis-4.0.1
RUN mkdir -p /usr/src/php/ext/redis; \
    curl -fsSL --ipv4 https://github.com/phpredis/phpredis/archive/4.0.1.tar.gz | tar xvz -C "/usr/src/php/ext/redis" --strip 1; \
    docker-php-ext-install redis
#RUN docker-php-ext-enable redis

# Other PHP7 Extensions
# mysql-client
RUN apt-get -y install libsqlite3-0 libsqlite3-dev default-mysql-client
RUN apt-get install -y sqlite3 
RUN apt-get install -y libxslt-dev
RUN apt-get install -y libmcrypt-dev
RUN apt-get install -y telnet 
# RUN apt-get install -y transport-https
# RUN apt-get install -y libmcrypt
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install xsl
RUN docker-php-ext-install soap
RUN docker-php-ext-install pdo_mysql 
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install mysqli
# mcrypt
# RUN docker-php-ext-install mcrypt-1.0.1 
RUN docker-php-ext-install sockets
RUN docker-php-ext-install curl
RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json


# zlib1g-dev
RUN apt-get -y install libzip-dev 
RUN docker-php-ext-install zip

RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

#RUN docker-php-ext-install mbstring
RUN docker-php-ext-install gettext

RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev
#RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 
RUN docker-php-ext-install -j$(nproc) gd
RUN apt-get update -qq && apt-get -y upgrade && apt-get update -qq

### Custom

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qq && apt-get -y upgrade && apt-get update -qq

# apt packages we might need:
RUN apt-get install -y -qq wget git ssh mariadb-client unzip vim rsync nginx jq

# extensions already in: ctype curl hash iconv pic ftp mbstring mysqlnd argon2 sodium pdo_sqlite sqlite3 libedit openssl zlib pear
# official Magento 2.4 required exts: bcmath ctype curl dom gd hash iconv intl mbstring openssl pdo_mysql simplexml soap xsl zip sockets

# for php extensions - not needed, all preinstalled except for gd:
RUN apt-get install -y -qq libjpeg62-turbo-dev libpng-dev libxml2-dev libxslt-dev libsodium-dev libzip-dev libfreetype6-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install bcmath gd intl pdo_mysql simplexml soap xsl zip sockets
RUN docker-php-ext-enable bcmath gd intl pdo_mysql simplexml soap xsl zip sockets

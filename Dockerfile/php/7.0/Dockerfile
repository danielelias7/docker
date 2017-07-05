FROM php:7.0-apache

RUN cp /dev/null /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ stretch main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ stretch main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ stretch-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ stretch-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian-security/ stretch/updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian-security/ stretch/updates main contrib non-free" >> /etc/apt/sources.list \
    && gpg --keyserver pool.sks-keyservers.net --recv-keys 1A7B6500 \
    && gpg -a --export 1A7B6500 | apt-key add - \
    && apt-get update

#Instalando Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.overload_var_dump=On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#Instalando apc
RUN printf "\n" | pecl install apcu \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# Instalando Paquetes faltantes
RUN apt-get update \
    && apt-get install -y libpq-dev zlib1g-dev libicu-dev g++ libxml2-dev memcached libmemcached-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \
    && docker-php-ext-install intl mbstring zip xml

COPY php.ini /usr/local/etc/php/

#Instalando memcached
RUN pecl install memcached \
    && docker-php-ext-enable memcached \
    && echo "extension=memcached.so" >> /usr/local/etc/php/php.ini


ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

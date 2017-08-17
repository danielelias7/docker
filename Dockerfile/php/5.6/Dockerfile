FROM php:5-apache

RUN cp /dev/null /etc/apt/sources.list \
    && echo "#Inicio del archivo /etc/apt/sources.list para estaciones de trabajo" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ jessie main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ jessie main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ jessie-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ jessie-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian-security/ jessie/updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian-security/ jessie/updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "#Backports" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ jessie-backports main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ jessie-backports main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "#Fin del archivo /etc/apt/sources.list" >> /etc/apt/sources.list \
    && gpg --keyserver pool.sks-keyservers.net --recv-keys 65558117 \
    && gpg -a --export 65558117 | apt-key add - \
    && gpg --keyserver pool.sks-keyservers.net --recv-keys 06C4AE2A \
    && gpg -a --export 06C4AE2A | apt-key add - \
    && apt-get update

#Instalando Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.overload_var_dump=On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#Instalando apc
RUN printf "\n" | pecl install apcu-4.0.11 \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# Instalando Paquetes faltantes
RUN apt-get update \
    && apt-get install -y libpq-dev zlib1g-dev libicu-dev g++ libicu52 php5-sybase freetds-bin freetds-common libpng-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql intl \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install gd

COPY php.ini /usr/local/etc/php/

ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

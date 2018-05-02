FROM php:7.2-apache

RUN cp /dev/null /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ stretch main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ stretch main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian/ stretch-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian/ stretch-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "" >> /etc/apt/sources.list \
    && echo "deb http://debian.salud.gob.sv/debian-security/ stretch/updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb-src http://debian.salud.gob.sv/debian-security/ stretch/updates main contrib non-free" >> /etc/apt/sources.list

#Instalando Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.overload_var_dump=On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && docker-php-ext-install opcache \
    && docker-php-ext-enable opcache \
    && echo "opcache.file_cache = /var/www/html/.opcache" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

#Instalando apc
RUN printf "\n" | pecl install apcu \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# Instalando Paquetes faltantes
RUN apt-get update \
    && apt-get install -y nano libpq-dev zlib1g-dev libicu-dev g++ libxml2-dev memcached libmemcached-dev \
    wkhtmltopdf freetds-bin freetds-common wget \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \
    && docker-php-ext-install intl mbstring zip xml

COPY php.ini /usr/local/etc/php/

#Instalando memcached
RUN pecl install memcached \
    && docker-php-ext-enable memcached

#Instalando Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite

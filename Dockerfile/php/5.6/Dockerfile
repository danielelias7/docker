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
RUN pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.overload_var_dump=On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#Instalando apc
RUN printf "\n" | pecl install apcu-4.0.11 \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# Instalando Paquetes faltantes
RUN apt-get update \
    && apt-get install -y libpq-dev zlib1g-dev libicu-dev g++ libicu52 php5-sybase freetds-bin freetds-common wget libxml2-dev \
    libpng-dev xvfb fontconfig fontconfig-config fonts-dejavu-core libfontconfig1 libjpeg62-turbo libxrender1 xfonts-75dpi \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql intl \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install soap \
    && docker-php-ext-install gd

RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.2/wkhtmltox-0.12.2_linux-jessie-amd64.deb \
    && dpkg --install wkhtmltox-0.12.2_linux-jessie-amd64.deb

COPY php.ini /usr/local/etc/php/

#Instalando Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

ADD 000-default.conf /etc/apache2/sites-available/000-default.conf

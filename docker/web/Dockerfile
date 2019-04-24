FROM php:5.6.28-apache
# FROM php:7.1-apache #php7をインストールしたい方はこちらを有効に

MAINTAINER docker-web

RUN echo "deb http://deb.debian.org/debian jessie main" > /etc/apt/sources.list &&\
    echo "deb http://security.debian.org jessie/updates main" >> /etc/apt/sources.list &&\
    apt-get update \
    && apt-get -y install zip unzip vim wget lsb-release libicu-dev mysql-client git php5-mcrypt libmcrypt-dev \
    && docker-php-ext-install pdo_mysql mysqli intl mcrypt
# mod_rewriteを 有効に
RUN a2enmod rewrite
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN ln -s /etc/apache2/mods-available/vhost_alias.load /etc/apache2/mods-enabled
RUN echo ServerName $HOSTNAME > /etc/apache2/conf-available/fqdn.conf && a2enconf fqdn
RUN a2enmod headers
RUN a2enmod include

# GD関連のインストール
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev
RUN docker-php-ext-install gd


COPY ./base.conf /etc/apache2/sites-available/base.conf
RUN a2ensite base.conf

COPY ./php.ini /usr/local/etc/php/

# ポートの解放
EXPOSE 80
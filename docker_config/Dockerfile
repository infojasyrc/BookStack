FROM ubuntu:18.04 AS builder

########### Docker for building git #################

RUN apt-get update && apt-get install -y \
    software-properties-common
RUN apt-add-repository -y ppa:git-core/ppa
RUN apt-get update && apt-get install -y git

########### Docker for Web application ###############

FROM php:7.3-apache
LABEL author="Jose Sal y Rosas"
LABEL author_email="infojasyrc@gmail.com"

COPY --from=builder /usr/bin/git /usr/bin/git

RUN apt-get update && apt-get install -y git-core \
    libzip-dev \
    zlib1g-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    wget \
    libldap2-dev \
    libtidy-dev

RUN docker-php-ext-install pdo pdo_mysql mbstring zip tidy \
    && docker-php-ext-configure ldap \
    && docker-php-ext-install ldap \
    && docker-php-ext-configure gd --with-freetype-dir=usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

COPY --from=composer:1.8 /usr/bin/composer /usr/bin/composer

RUN a2enmod rewrite

FROM php:8.1.2-apache

ENV LANG C.UTF-8
ENV COMPOSER_ALLOW_SUPERUSER 1

EXPOSE 5173

COPY php.ini /usr/local/etc/php/
COPY *.conf /etc/apache2/sites-enabled/

RUN apt-get update && apt-get install -y gnupg curl

RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list

RUN apt -y update \
    && apt -y upgrade \
    && apt -y install \
            git \
            libpq-dev \
            nodejs \
            unzip \
            zip

RUN apt -y install \
            zlib1g-dev  \
            libheif-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            librsvg2-dev \
            libwebp-dev \
            libxpm-dev \
            libmagickwand-dev \
            libzip-dev

RUN pecl install imagick \
    && docker-php-ext-enable imagick

RUN docker-php-ext-install pdo_pgsql zip

RUN a2enmod rewrite

COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
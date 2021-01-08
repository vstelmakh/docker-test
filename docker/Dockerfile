FROM php:7.4-fpm AS php

RUN apt-get update \
    && apt-get install -y \
        openssl \
        git \
        unzip \
        libicu-dev \
        libzip-dev \
    && apt-get autoremove -y -f \
    && apt-get clean -y \
    && rm -rf /tmp/* /root/.cache /var/log/* /var/lib/apt/lists/*

RUN docker-php-ext-install \
    intl \
    opcache \
    pdo_mysql \
    zip

RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
    && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm composer-setup.php*

RUN mkdir -p /app/public \
    && chown -R www-data:www-data /var/www \
    && chown -R www-data:www-data /app

WORKDIR /app

#--------------------------------------------------#

FROM nginx:stable AS nginx
WORKDIR /usr/share/nginx/html
RUN echo -n "Long running process..." \
    && sleep 10 \
    && echo " done"
RUN echo "It works!" > index.html

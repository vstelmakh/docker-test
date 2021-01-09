FROM php:7.4-fpm AS php

RUN apt-get update \
    && apt-get install -y \
        git \
        unzip \
    && apt-get autoremove -y -f \
    && apt-get clean -y \
    && rm -rf /tmp/* /root/.cache /var/log/* /var/lib/apt/lists/*

RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
    && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm composer-setup.php*

RUN mkdir -p /app/public \
    && chown -R www-data:www-data /var/www \
    && chown -R www-data:www-data /app

WORKDIR /app

#--------------------------------------------------#

FROM php as php_prod

COPY --chown=www-data:www-data ./app /app

RUN composer install --prefer-dist --no-progress --no-interaction --no-dev --optimize-autoloader \
    && chown -R www-data:www-data /app/vendor

#--------------------------------------------------#

FROM php AS php_dev

RUN touch test.txt

COPY --chown=www-data:www-data ./app /app

RUN composer install --prefer-dist --no-progress --no-interaction \
    && chown -R www-data:www-data /app/vendor


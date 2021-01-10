FROM php:7.4-fpm AS php_base

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

FROM php_base as php_prod

COPY --chown=www-data:www-data ./app /app

RUN composer install --prefer-dist --no-progress --no-interaction --no-dev --optimize-autoloader \
    && chown -R www-data:www-data /app/vendor

#--------------------------------------------------#

FROM php_base AS php_dev

RUN echo -n "Long running process..." \
    && sleep 10 \
    && echo " done"

# No files needed in dev image as bind mount will be used


FROM 106624261994.dkr.ecr.eu-west-2.amazonaws.com/wifeplanet-backend-test:composer2.4 as build
COPY . /app/
RUN composer install --prefer-dist --optimize-autoloader --no-interaction
RUN composer dump-autoload

FROM 106624261994.dkr.ecr.eu-west-2.amazonaws.com/wifeplanet-backend-test:php8.0-apache-buster as production
RUN docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install pdo pdo_mysql
COPY docker/php/conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini
COPY --from=build /app /var/www/html
COPY docker/apache/000-default.conf /etc/apache2/sites-available/000-default.conf
RUN chmod 777 -R /var/www/html/storage/ && chown -R www-data:www-data /var/www/ && a2enmod rewrite
#CMD ["php","artisan","migrate:refresh","--seed"]

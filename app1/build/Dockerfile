FROM gargximran/php8-with-composer:1.3

WORKDIR /var/www/app/


COPY . .

RUN mv /var/www/app/000-default.conf /etc/apache2/sites-enabled/

RUN mv /var/www/app/.env.example /var/www/app/.env

RUN chown -R www-data:www-data /var/www/app

EXPOSE 80

RUN composer update --no-dev

RUN php artisan key:generate


ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]


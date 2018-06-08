FROM php:5.6-apache

COPY docker/php/php.ini-development /usr/local/etc/php/php.ini

RUN apt-get update \
 && apt-get install -y git zlib1g-dev libicu-dev libmcrypt-dev libreadline-dev libxml2-dev libzip-dev g++ \
 && docker-php-ext-configure intl \
 && docker-php-ext-install mbstring mcrypt xml zip pdo_mysql intl \
 && a2enmod rewrite \
 && sed -i 's!/var/www/html!/var/www!g' /etc/apache2/sites-available/000-default.conf \
 && cd /var/www && rm -r html \
 && curl -sS https://getcomposer.org/installer \
  | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www

# Build can be given uid as an argument.  It should match with the UID
# of the user who is running the build and who will be executing the
# container.  This ensures that files written to host volumes will
# have same owner as the person executing the container.
ARG uid

RUN useradd --uid $uid webdev
RUN chmod -R 775 /var/www
RUN usermod -a -G www-data webdev
RUN chown www-data:webdev -R /var/www

FROM php:7-apache

MAINTAINER Alexander Bauer <alex.ab888@gmail.com>

# Custom PHP Settings
ADD php.ini /usr/local/etc/php/

# Custom Apache VHosts
ADD intranet.conf /etc/apache2/sites-available/

# Enable VHosts
RUN ln -s /etc/apache2/sites-available/intranet.conf /etc/apache2/sites-enabled/intranet.conf

RUN apt-get update \
&& apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng12-dev libmcrypt-dev \
libcurl3-gnutls-dev libldb-dev libldap2-dev libxml2-dev \
git unzip curl vim

RUN ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so \
    && ln -s /usr/lib/x86_64-linux-gnu/liblber.so /usr/lib/liblber.so

# Install PHP-Extensions
RUN docker-php-ext-install pdo_mysql mysqli mbstring gd iconv mcrypt curl ldap soap


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
RUN "date"

# install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/apps

RUN usermod -u 1000 www-data

EXPOSE 9001

RUN a2enmod rewrite && service apache2 restart

CMD ["apache2-foreground"]

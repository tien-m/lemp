FROM php:7.1-fpm

# Install modules
RUN apt-get update && apt-get install -y \
	wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libicu-dev \
	libssl-dev \
            --no-install-recommends

RUN docker-php-ext-install mcrypt bcmath zip intl mbstring pdo_mysql exif \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN pecl install -o -f xdebug \
    && rm -rf /tmp/pear

COPY ./php.ini /usr/local/etc/php/
COPY ./www.conf /usr/local/etc/php/

#RUN  wget -O /usr/local/bin/drush http://files.drush.org/drush.phar 
#RUN  chmod +x /usr/local/bin/drush 

 # Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

 # install mcrypt for xhgui
 RUN apt-get install -y mcrypt 

 # install pkg-config for pecl
 RUN apt-get install -y pkg-config

 # install Tideways profiler extension
 RUN git clone https://github.com/tideways/php-profiler-extension.git
 RUN cd php-profiler-extension && phpize && ./configure && make && make install

 # install mongodb extension for xhgui
 RUN pecl install mongodb

 # install xhgui
 RUN git clone https://github.com/perftools/xhgui.git /xhgui


 # init container by running installation of composer for xhgui and staring fpm 
 CMD composer install -d /xhgui && fpm-start

RUN apt-get purge -y g++ \
    && apt-get autoremove -y \
    && rm -r /var/lib/apt/lists/* \
    && rm -rf /tmp/*

RUN usermod -u 1000 www-data

EXPOSE 9000
CMD ["php-fpm"]


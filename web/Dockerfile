FROM php:5.6-cli

RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php \
  && php composer-setup.php -- --install-dir=/usr/local/bin --filename=composer \
  && rm -f 'composer-setup.php'

RUN sed -i -e 's#//httpredir\.debian\.org#//ftp.jp.debian.org#g'  /etc/apt/sources.list \
    && echo "Asia/Tokyo" > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      git \
      zlib1g-dev \
    && rm -r /var/lib/apt/lists/*

COPY ./php.ini /usr/local/etc/php
RUN docker-php-ext-install \
  pdo \
  pdo_mysql \
  pcntl \
  zip

RUN pecl install uri_template igbinary && \
  docker-php-ext-enable uri_template && \
  docker-php-ext-enable igbinary
  

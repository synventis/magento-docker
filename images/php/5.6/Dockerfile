FROM php:5.6-fpm
MAINTAINER Marc Sladek <marc.sladek@synventis.com>

ARG APP_ID=1000
RUN groupadd -g "$APP_ID" app \
  && useradd -g "$APP_ID" -u "$APP_ID" -d /var/www -s /bin/bash app

RUN mkdir -p /etc/nginx/html /var/www/html /sock \
  && chown -R app:app /etc/nginx /var/www /usr/local/etc/php/conf.d /sock

RUN apt-get update && apt-get install -y \
    cron \
    git \
    gzip \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libxslt1-dev \
    lsof \
    mysql-client \
    vim \
    zip \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install \
    bcmath \
    gd \
    intl \
    mbstring \
    mcrypt \
    opcache \
    pdo_mysql \
    soap \
    xsl \
    zip

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

RUN printf '* *\t* * *\tapp\t%s/usr/local/bin/php /var/www/html/cron.php\n#\n' >> /etc/crontab

COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/php-fpm.conf /usr/local/etc/
COPY bin/cronstart /usr/local/bin/

USER app:app
VOLUME /var/www
WORKDIR /var/www/html
EXPOSE 9001

FROM php:7.4-fpm
RUN set -xe; \
    apt-get update -yqq && \
    apt-get install -yqq --no-install-recommends \
      apt-utils vim autossh gettext git redis net-tools procps \
      default-mysql-client \
      # for gd
      libfreetype6-dev \
      libjpeg62-turbo-dev \
      libpng-dev \
      libwebp-dev \
      libxpm-dev \
      # for ImageMagick
      libmagickwand-dev \
      # for zip
      libzip-dev zip unzip \
      # for oniguruma
      libonig-dev \
    && docker-php-ext-install bcmath gettext mbstring mysqli pdo pdo_mysql zip \
    && docker-php-ext-configure mbstring --disable-mbregex \
    && docker-php-ext-configure zip \
    # gd
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    # imagick
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    # xlswriter
    && pecl install xlswriter \
    && docker-php-ext-enable xlswriter \
    # exif
    && docker-php-ext-install exif \
    && docker-php-ext-enable exif \
    # install composer
    && cd '/' \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql


RUN pecl install redis && docker-php-ext-enable redis

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# VOLUME ["/var/www", "/data"]
WORKDIR /var/www
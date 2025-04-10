# @description php image base on the alpine 镜像更小. 本镜像用于开发，所以安装了常用工具
#                       some information
# ------------------------------------------------------------------------------------
# @link https://hub.docker.com/_/alpine/      alpine image
# @link https://hub.docker.com/_/php/         php image
# @link https://github.com/docker-library/php php dockerfiles
# ------------------------------------------------------------------------------------
# @build-example docker build . -f alphp-dev.Dockerfile -t swoft/alphp:dev
#

FROM swoft/alphp:cli
LABEL maintainer="Elliot" version="1.2"

ENV NAME="docker_php"

WORKDIR /var/www

RUN set -ex \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN set -ex \
        && php -m \
        # install some tools
        && apk update \
        && apk add --no-cache \
            build-base libgit2-dev php7-dev php7-fpm php7-pcntl php7-bcmath php7-mysqli php7-xmlwriter php7-pear php7-xmlreader php7-pgsql php7-pdo_pgsql php7-soap\
            vim wget autossh openssh zsh net-tools git zip unzip apache2-utils mysql-client redis \
        && apk del --purge *-dev \
        && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
        # install latest composer
        # && wget https://getcomposer.org/download/1.10.5/composer.phar \
        && wget https://mirrors.aliyun.com/composer/composer.phar \
        && mv composer.phar /usr/local/bin/composer \
	&& chmod +x /usr/local/bin/composer \
        # - config PHP-FPM
        && cd /etc/php7 \
        && { \
            echo "[global]"; \
            echo "pid = /var/run/php-fpm.pid"; \
            echo "[www]"; \
            echo "user = www-data"; \
            echo "group = www-data"; \
        } | tee php-fpm.d/custom.conf \
        # config site
        && chown -R www-data:www-data /var/www \
        && { \
            echo "#!/bin/sh"; \
            echo "php-fpm7 -F"; \
        } | tee /run.sh \
        && chmod 755 /run.sh

# install oh my zsh
RUN wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | zsh || true

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

VOLUME ["/var/www", "/data"]

EXPOSE 9501 80

CMD /run.sh

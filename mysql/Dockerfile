FROM alpine:3.7
MAINTAINER Elliot <Mr.Elliot.SE@gmail.com>
#3.8 mysql版本高 不兼容

ENV NAME="docker_mysql"
# See https://github.com/dockerfile/mariadb/issues/3
ENV TERM dumb

RUN mkdir /docker-entrypoint-initdb.d

RUN apk add --update mysql mysql-client bash pwgen tzdata && \
    rm -f /var/cache/apk/* && \
    mkdir -p /etc/mysql/conf.d \
    && { \
        echo '[mysqld]'; \
        echo 'skip-host-cache'; \
        echo 'skip-name-resolve'; \
        echo 'user = mysql'; \
        echo 'datadir = /var/lib/mysql'; \
        echo '!includedir /etc/mysql/conf.d/'; \
    } > /etc/mysql/my.cnf


# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod a+x /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 3306
CMD ["mysqld"]

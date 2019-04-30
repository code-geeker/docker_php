FROM alpine:3.8

MAINTAINER Elliot <Mr.Elliot.SE@gmail.com>

ENV NAME="docker_mongodb"

RUN apk add --no-cache mongodb

VOLUME /data/db
EXPOSE 27017 28017

COPY run.sh /root
ENTRYPOINT [ "/root/run.sh" ]
CMD [ "mongod", "--bind_ip", "0.0.0.0" ]

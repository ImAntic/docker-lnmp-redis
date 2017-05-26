FROM redis:3.2
MAINTAINER antic antic.zhu@gmail.com

ENV TZ "Asia/Shanghai"

ADD redis/config/redis.conf /etc/redis.conf
CMD [ "redis-server", "/etc/redis.conf" ]

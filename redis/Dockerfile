FROM redis:6.2.3-alpine3.13

RUN mkdir -p /var/run/redis/ /var/log/redis && chown -R redis.redis /var/run/redis && chown -R redis.redis /var/log/redis 

COPY redis.conf /etc/redis.conf

CMD ["redis-server", "/etc/redis.conf"]

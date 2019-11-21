FROM redis:5-alpine

ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 5000
ENV SENTINEL_FAILOVER 10000
ENV SENTINEL_PORT 26000
ENV SENTINEL_PAR_SYNC 1

ADD sentinel.conf /etc/redis/sentinel.conf
RUN chown redis:redis /etc/redis/sentinel.conf

ADD entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
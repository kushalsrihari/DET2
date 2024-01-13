FROM mysql:5.7

VOLUME /data

ENV MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    MYSQL_DATABASE=cities \
    MYSQL_USER=shipping \
    MYSQL_PASSWORD=secret

# change datadir entry in /etc/mysql/my.cnf
COPY config.sh /root/
RUN chmod +x /root/config.sh && dos2unix /root/config.sh

# Run the script using bash
RUN /bin/bash -c /root/config.sh

COPY scripts/* /docker-entrypoint-initdb.d/

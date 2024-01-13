FROM mysql:5.7

VOLUME /data

ENV MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    MYSQL_DATABASE=cities \
    MYSQL_USER=shipping \
    MYSQL_PASSWORD=secret

# change datadir entry in /etc/mysql/my.cnf
COPY config.sh /root/
RUN chmod +x /root/config.sh && tr -d '\r' < /root/config.sh > /root/config_unix.sh && mv /root/config_unix.sh /root/config.sh && chown root:root /root/config.sh

# Run the script using bash
RUN /bin/bash -c /root/config.sh

COPY scripts/* /docker-entrypoint-initdb.d/

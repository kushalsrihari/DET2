FROM mysql:5.7

VOLUME /data

ENV MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    MYSQL_DATABASE=cities \
    MYSQL_USER=shipping \
    MYSQL_PASSWORD=secret

# change datadir entry in /etc/mysql/my.cnf
COPY config.sh /root/config.sh
RUN chmod +x /root/config.sh && tr -d '\r' < /root/config.sh > /root/config_unix.sh && mv /root/config_unix.sh /root/config.sh

# Set the script as the entry point, making it executable during container startup
ENTRYPOINT ["/bin/bash", "/root/config.sh"]

COPY scripts/* /docker-entrypoint-initdb.d/

FROM busybox
ENV CONFD_VERSION 0.10.0
RUN \
    curl -s -L https://github.com/kelseyhightower/confd/releases/download/v$CONFD_VERSION/confd-$CONFD_VERSION-linux-amd64 -o confd; \
    chmod +x confd

# Create directories
RUN mkdir -p /etc/confd/conf.d
RUN mkdir -p /etc/confd/templates

VOLUME ["/etc/conf"]
ADD etc/confd/ /etc/confd

CMD /usr/bin/confd -interval=60 -node=http://$COREOS_PRIVATE_IPV4:4001

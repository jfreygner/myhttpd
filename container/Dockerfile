FROM registry.redhat.io/ubi9/ubi-minimal

ENV HTTPD_PORT=8080

ARG LOGDIR=/var/log/httpd

LABEL description="My first HTTPD Container Image" \
      maintainer="J. Freygner" \
      version="1.1"

RUN microdnf -y --nodocs install httpd mod_ssl && \
    microdnf -y update && \
    microdnf clean all && \
    sed -i "/^Listen/s/80/$HTTPD_PORT/" /etc/httpd/conf/httpd.conf && \
    sed -i "/^Listen/s/443/8443/" /etc/httpd/conf.d/ssl.conf && \
    echo "<h1>This is a testpage from a containerimage build with podman</h1>" > /var/www/html/index.html && \
    chown -R 1001:0 $LOGDIR /var/run/httpd && \
    chmod -R g=u $LOGDIR /var/run/httpd

ADD files/container-entrypoint /usr/local/sbin

EXPOSE $HTTPD_PORT

VOLUME /var/www/html

# next two commands are only for analysing and troubleshooting
# remove for production images 
RUN microdnf -y --nodocs install procps-ng iproute iputils && \
    microdnf clean all

USER 1001

ENTRYPOINT ["/usr/local/sbin/container-entrypoint"]

CMD ["/usr/sbin/httpd","-DFOREGROUND"]

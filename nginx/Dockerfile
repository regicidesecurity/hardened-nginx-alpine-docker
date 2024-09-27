FROM alpine:latest

ENV NGINX_VERSION stable-1.26

ARG CONF=nginx.conf

RUN apk --update add openssl-dev pcre-dev zlib-dev wget build-base git 
RUN git clone https://github.com/nginx/nginx.git

RUN cd nginx && git checkout ${NGINX_VERSION} && ./auto/configure \
    --with-http_ssl_module \
    --with-http_gzip_static_module \
    --prefix=/etc/nginx \
    --http-log-path=/var/log/nginx/access.log \
    --error-log-path=/var/log/nginx/error.log \
    --sbin-path=/usr/local/sbin/nginx && \
    make && \
    make install

COPY ${CONF} /etc/nginx/conf/nginx.conf

RUN chmod 550 /usr/local/sbin/nginx
RUN chmod 770 /etc/nginx/conf
RUN chmod 660 /etc/nginx/conf/nginx.conf
RUN chmod 750 /var/log/nginx
RUN chmod 1600 /etc/nginx/html

VOLUME ["/var/log/nginx"]

WORKDIR /etc/nginx

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
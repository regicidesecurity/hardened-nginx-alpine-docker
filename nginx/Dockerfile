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

VOLUME ["/var/log/nginx"]

WORKDIR /etc/nginx

EXPOSE 80 443

COPY ${CONF} /etc/nginx/conf/nginx.conf

CMD ["nginx", "-g", "daemon off;"]
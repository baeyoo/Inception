FROM alpine:3.13

RUN	apk update && apk upgrade && apk add --no-cache \
        nginx \
        openssl

# nginx SSL
RUN mkdir /etc/nginx/ssl
RUN openssl req -newkey rsa:4096 -x509 -sha256 -days 365 -nodes \
        -out /etc/nginx/ssl/mobrycki.pem \
        -keyout /etc/nginx/ssl/mobrycki.key \
        -subj "/C=IT/ST=Rome/L=Rome/O=42 School/OU=mobrycki/CN=mobrycki/"

# nginx config
RUN mkdir -p /run/nginx
COPY conf/default.conf /etc/nginx/conf.d/default.conf

#run nginx in forground to prevent container stopping
ENTRYPOINT ["nginx", "-g", "daemon off;"]
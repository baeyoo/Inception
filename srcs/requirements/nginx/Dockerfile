#FROM debian:buster
#RUN apt-get update && apt-get upgrade -y
#RUN apt-get install nginx -y
#WORKDIR /tmp/
#ENV BUILDDIR=/tmp/build/
#RUN mkdir ${BUILDDIR}
#COPY src/ ${WORKDIR}
#RUN chmod 770 mkcert-v1.4.3-linux-amd64 && \
#	./mkcert-v1.4.3-linux-amd64 -install && \
#	./mkcert-v1.4.3-linux-amd64 localhost && \
#	mkdir -p /etc/nginx/ssl/ && \
#	mv localhost.pem /etc/nginx/ssl/ && \
#	mv localhost-key.pem /etc/nginx/ssl/ && \
#	chmod -R 770 /etc/nginx/ssl/
#RUN rm -rf /etc/nginx/sites-enabled/* && \
#	mv nginx.conf /etc/nginx/sites-available/nginx.conf && \
#	ln -fs /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/
#EXPOSE 443
#RUN chmod -R 777 ${BUILDDIR}
# -g && daemon off: makes it run in foreground(primo piano)
#CMD cp -a ${BUILDDIR}. /var/www/ && \
#	nginx -g 'daemon off;'
################################################################################
# NGINX with TLSv1.2 or TLSv1.3 only.

#Specify which mirror source to use the container devra tourner avec Debian Buster.
FROM debian:buster  

#Specify the next working path as /APP
WORKDIR /APP

#Copy all current files including folders to the /APP folder in the container
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf


# Create website folder 
RUN mkdir -p / var / www / website

RUN apt-get update \
    && apt-get -y install openssl \      
    && apt-get -y install wget \      
    && apt-get -y install nginx

# Get SSL certification
RUN mkdir /etc/nginx/ssl \
&& openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout /etc/nginx/ssl/mobryckikey.key \
    -out /etc/nginx/ssl/mobrycki.csr -subj "/C=IT/ST=Rome/L=Rome/O=42/OU=IT/CN=mobrycki/"

#Configure nginx
RUN rm -rf /etc/nginx/sites-enabled/default \
    && cp ./nginx.conf /etc/nginx/sites-available \
    && ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/ \
    && chown -R www-data /var/www/* \
    && chmod -R 755 /var/www/*

EXPOSE 443
#Command to be executed when the container starts


CMD ["nginx", "-g", "daemon off;"]
# CMD sh ./init.sh
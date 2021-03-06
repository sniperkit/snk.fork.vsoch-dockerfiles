FROM wodby/drupal-php:7.1  
RUN set -xe && \  
mv /usr/local/bin/actions.mk /usr/local/bin/drupal-php.mk && \  
mkdir /usr/src/drupal && \  
chown www-data:www-data /usr/src/drupal && \  
which composer && \  
which drush && \  
which php && \  
php -m  
  
RUN apk add --update --no-cache \  
ca-certificates \  
openssl && \  
rm -rf /var/cache/apk/*  
  
# Build nghttp2 libraries from source  
ARG NGHTTP2_RELEASE  
ENV NGHTTP2_RELEASE ${NGHTTP2_RELEASE:-v1.21.1}  
  
RUN mkdir -p /usr/local/src/nghttp2 && cd /usr/local/src/nghttp2 && \  
apk add --update --no-cache --virtual build-dependencies \  
autoconf automake g++ libtool make tar && \  
wget -qO- https://github.com/nghttp2/nghttp2/archive/${NGHTTP2_RELEASE}.tar.gz
| \  
tar -xzf - --directory=/usr/local/src/nghttp2 --strip-components=1 && \  
autoreconf -i && automake && autoconf && \  
./configure && \  
make && make install && \  
apk del build-dependencies && rm -rf /usr/local/src/nghttp2 /var/cache/apk/*  
  
# Build curl from source  
ARG CURL_RELEASE  
ENV CURL_RELEASE ${CURL_RELEASE:-curl-7.53.1}  
  
RUN mkdir -p /usr/local/src/curl && cd /usr/local/src/curl && \  
apk add --update --no-cache --virtual build-dependencies \  
autoconf automake g++ libtool make openssl-dev tar && \  
wget -qO- https://curl.haxx.se/download/${CURL_RELEASE}.tar.gz | \  
tar -xzf - --directory=/usr/local/src/curl --strip-components=1 && \  
./buildconf && \  
./configure --enable-ipv6 --enable-threaded-resolver \  
\--with-nghttp2=/usr/local \--with-ssl && \  
make && make install && \  
apk del build-dependencies && rm -rf /usr/local/src/curl /var/cache/apk/*  
  
COPY init /docker-entrypoint-init.d/


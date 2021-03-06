FROM alpine:3.7  
  
ENV HAPROXY_MAJOR 1.8  
ENV HAPROXY_VERSION 1.8.2  
ENV HAPROXY_MD5 5e72829793e163bea93da1df6b4aaa1e  
  
# https://www.lua.org/ftp/#source  
ENV LUA_VERSION=5.3.4 \  
LUA_SHA1=79790cfd40e09ba796b01a571d4d63b52b1cd950  
  
# see http://sources.debian.net/src/haproxy/jessie/debian/rules/ for some
helpful navigation of the possible "make" arguments  
RUN set -x \  
\  
&& apk add --no-cache --virtual .build-deps \  
ca-certificates \  
gcc \  
libc-dev \  
linux-headers \  
make \  
openssl \  
openssl-dev \  
pcre-dev \  
readline-dev \  
tar \  
zlib-dev \  
\  
# install Lua  
&& wget -O lua.tar.gz "https://www.lua.org/ftp/lua-$LUA_VERSION.tar.gz" \  
&& echo "$LUA_SHA1 *lua.tar.gz" | sha1sum -c \  
&& mkdir -p /usr/src/lua \  
&& tar -xzf lua.tar.gz -C /usr/src/lua --strip-components=1 \  
&& rm lua.tar.gz \  
&& make -C /usr/src/lua -j "$(getconf _NPROCESSORS_ONLN)" linux \  
&& make -C /usr/src/lua install \  
# put things we don't care about into a "trash" directory for purging  
INSTALL_BIN='/usr/src/lua/trash/bin' \  
INSTALL_CMOD='/usr/src/lua/trash/cmod' \  
INSTALL_LMOD='/usr/src/lua/trash/lmod' \  
INSTALL_MAN='/usr/src/lua/trash/man' \  
# ... and since it builds static by default, put those bits somewhere we can
purge after we build haproxy  
INSTALL_INC='/usr/local/lua-install/inc' \  
INSTALL_LIB='/usr/local/lua-install/lib' \  
&& rm -rf /usr/src/lua \  
\  
# install HAProxy  
&& wget -O haproxy.tar.gz
"http://www.haproxy.org/download/${HAPROXY_MAJOR}/src/haproxy-${HAPROXY_VERSION}.tar.gz"
\  
&& echo "$HAPROXY_MD5 *haproxy.tar.gz" | md5sum -c \  
&& mkdir -p /usr/src/haproxy \  
&& tar -xzf haproxy.tar.gz -C /usr/src/haproxy --strip-components=1 \  
&& rm haproxy.tar.gz \  
\  
&& makeOpts=' \  
TARGET=linux2628 \  
USE_LUA=1 LUA_INC=/usr/local/lua-install/inc LUA_LIB=/usr/local/lua-
install/lib \  
USE_OPENSSL=1 \  
USE_PCRE=1 PCREDIR= \  
USE_ZLIB=1 \  
' \  
&& make -C /usr/src/haproxy -j "$(getconf _NPROCESSORS_ONLN)" all $makeOpts \  
&& make -C /usr/src/haproxy install-bin $makeOpts \  
\  
# purge the remnants of our static Lua  
&& rm -rf /usr/local/lua-install \  
\  
&& mkdir -p /usr/local/etc/haproxy \  
&& cp -R /usr/src/haproxy/examples/errorfiles /usr/local/etc/haproxy/errors \  
&& rm -rf /usr/src/haproxy \  
\  
&& runDeps="$( \  
scanelf --needed --nobanner --recursive /usr/local \  
| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \  
| sort -u \  
| xargs -r apk info --installed \  
| sort -u \  
)" \  
&& apk add --virtual .haproxy-rundeps $runDeps \  
&& apk del .build-deps  
  
RUN apk add --update --no-cache diffutils bash gawk sed grep bc coreutils curl
jq vim keepalived openssl \  
&& rm -rf /var/cache/apk/*  
  
COPY config config  
  
RUN chmod -R +x config \  
&& touch /var/run/haproxy.pid \  
&& ln -sf /proc/1/fd/1 /keepalived.log  
  
#RUN set -x ; \  
# addgroup -g 499 -S app ; \  
# adduser -u 499 -D -S -G app app && exit 0 ; exit 1  
#RUN chown -R app:app /config  
#USER app  
  
CMD ["bash", "-c", "/config/endpoints_script.sh"]  
#CMD ["bash", "-c", "/config/dummy.sh"]  


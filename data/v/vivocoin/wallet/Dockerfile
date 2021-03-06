FROM alpine:latest as build  
WORKDIR /root/build  
# DB4.8  
RUN apk update && \  
apk add \  
build-base \  
make && \  
wget http://download.oracle.com/berkeley-db/db-4.8.30.tar.gz && \  
tar xf db-4.8.30.tar.gz && \  
cd db-4.8.30/build_unix && \  
../dist/configure \  
\--enable-cxx \  
\--with-pic \  
\--enable-shared \  
\--disable-static \  
\--prefix=/usr && \  
make -j $(nproc) && \  
make install  
# VIVO wallet  
RUN apk add \  
git \  
boost-dev \  
autoconf \  
automake \  
libtool \  
libevent-dev \  
zeromq-dev \  
libressl-dev && \  
git clone https://github.com/vivocoin/vivo && \  
cd vivo && \  
chmod +x share/genbuild.sh && \  
sh autogen.sh && \  
./configure \  
\--disable-static \  
\--disable-shared \  
\--disable-tests \  
\--disable-bench \  
\--without-miniupnpc \  
\--without-libs \  
\--without-utils \  
\--with-gui=no \  
\--with-daemon \  
\--prefix=/root/build/vivo/install && \  
make -j $(nproc) && \  
make install && \  
strip \  
\--strip-unneeded \  
-R .comment \  
-R .GCC.command.line \  
-R .note.gnu.gold-version \  
/root/build/vivo/install/bin/*  
  
FROM alpine:latest as assemble  
RUN apk update && \  
apk --no-cache add \  
libevent \  
zeromq \  
libressl && \  
adduser -s /bin/false -D vivo  
COPY --from=build \  
/usr/lib/libboost_system.so.1.62.0 \  
/usr/lib/libboost_filesystem.so.1.62.0 \  
/usr/lib/libboost_program_options-mt.so.1.62.0 \  
/usr/lib/libboost_system-mt.so.1.62.0 \  
/usr/lib/libboost_thread-mt.so.1.62.0 \  
/usr/lib/libboost_chrono-mt.so.1.62.0 \  
/usr/lib/libdb-4.8.so \  
/usr/lib/libdb_cxx-4.8.so \  
/usr/lib/  
WORKDIR /opt/wallet/vivo  
COPY --from=build /root/build/vivo/install/bin/vivod .  
COPY init.sh bootstrap.sh function.sh ./  
RUN chmod 755 init.sh bootstrap.sh  
  
FROM scratch  
COPY --from=assemble / /  
RUN chown vivo:vivo -R /home/vivo  
EXPOSE 9998 12845  
VOLUME /vivo  
ENV SECRET_OVERRIDE=0 VIVO_RPCUSER=vivo VIVO_RCPPASSWORD=vivo
SECRET_OVERRIDE=0  
ENTRYPOINT [ "/opt/wallet/vivo/init.sh", "-printtoconsole" ]  


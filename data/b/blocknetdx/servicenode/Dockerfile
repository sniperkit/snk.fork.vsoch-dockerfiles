# Build via docker:  
# docker build --build-arg cores=8 -t blocknetdx/servicenode:3.9.22 .  
# docker build --build-arg cores=32 -t blocknetdx/servicenode:3.9.22 .  
FROM ubuntu:trusty  
  
ARG cores=32  
ENV ecores=$cores  
  
RUN apt update \  
&& apt install -y --no-install-recommends \  
software-properties-common \  
ca-certificates \  
wget curl git python vim \  
&& apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*  
  
RUN add-apt-repository ppa:bitcoin/bitcoin \  
&& apt update \  
&& apt install -y --no-install-recommends \  
build-essential libtool autotools-dev bsdmainutils \  
libevent-dev autoconf automake pkg-config libssl-dev \  
libboost-system-dev libboost-filesystem-dev libboost-chrono-dev \  
libboost-program-options-dev libboost-test-dev libboost-thread-dev \  
libdb4.8-dev libdb4.8++-dev libgmp-dev libminiupnpc-dev libzmq3-dev \  
&& apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*  
  
# Build berkeleydb4.8  
RUN mkdir -p /tmp/berkeley \  
&& cd /tmp/berkeley \  
&& wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz' \  
&& [ "$(printf
'12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef
db-4.8.30.NC.tar.gz' | sha256sum -c)" = "db-4.8.30.NC.tar.gz: OK" ] || $(echo
"Berkeley DB 4.8 sha256sum failed"; exit 1) \  
&& tar -xzvf db-4.8.30.NC.tar.gz \  
&& cd db-4.8.30.NC/build_unix/ \  
&& ../dist/configure --enable-cxx --disable-shared --with-pic
--prefix=/tmp/berkeley \  
&& make install  
  
# Build source  
RUN mkdir -p /opt/blocknetdx \  
&& mkdir -p /opt/blockchain/config \  
&& mkdir -p /opt/blockchain/data \  
&& ln -s /opt/blockchain/config /root/.blocknetdx \  
&& cd /opt/blocknetdx \  
&& git clone --depth 1 --branch v3.9.22
https://github.com/BlocknetDX/BlockDX.git \  
&& cd /opt/blocknetdx/BlockDX \  
&& chmod +x ./autogen.sh \  
&& ./autogen.sh \  
&& ./configure LDFLAGS="-L/tmp/berkeley/lib/"
CPPFLAGS="-I/tmp/berkeley/include/" --without-gui --enable-hardening \  
&& echo "Building with cores: $ecores" \  
&& make -j$ecores \  
&& make install \  
&& rm -rf /opt/blocknetdx/ /tmp/berkeley/*  
  
# Write default blocknetdx.conf (can be overridden on commandline)  
RUN echo "datadir=/opt/blockchain/data \n\  
\n\  
dbcache=256 \n\  
maxmempool=512 \n\  
\n\  
port=41412 # testnet: 41474 \n\  
rpcport=41414 # testnet: 41419 \n\  
\n\  
listen=1 \n\  
server=1 \n\  
maxconnections=100 \n\  
logtimestamps=1 \n\  
logips=1 \n\  
\n\  
rpcallowip=127.0.0.1 \n\  
rpctimeout=15 \n\  
rpcclienttimeout=15" > /opt/blockchain/config/blocknetdx.conf  
  
WORKDIR /opt/blockchain/  
VOLUME ["/opt/blockchain/config", "/opt/blockchain/data"]  
  
# Port, RPC, Test Port, Test RPC  
EXPOSE 41412 41414 41474 41419  
  
CMD ["blocknetdxd", "-daemon=0", "-server=0"]  


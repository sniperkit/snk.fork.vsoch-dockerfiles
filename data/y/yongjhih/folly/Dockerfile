FROM ubuntu:14.04  
  
ADD . /folly  
  
WORKDIR /folly/folly  
ENV LD_LIBRARY_PATH /usr/local/lib  
  
RUN apt-get update && apt-get install -y \  
g++ \  
automake \  
autoconf \  
autoconf-archive \  
libtool \  
libboost-all-dev \  
libevent-dev \  
libdouble-conversion-dev \  
libgoogle-glog-dev \  
libgflags-dev \  
liblz4-dev \  
liblzma-dev \  
libsnappy-dev \  
make \  
zlib1g-dev \  
binutils-dev \  
libjemalloc-dev \  
libssl-dev \  
libiberty-dev && \  
autoreconf -ivf && ./configure && make && make install && \  
apt-get clean && \  
rm -rf /var/lib/apt/lists/*  


FROM kallikrein/ubuntu:14.04  
MAINTAINER Thomas Charlat "thomas.charlat@gmail.com"  
RUN curl https://nodejs.org/dist/v6.10.3/node-v6.10.3-linux-x64.tar.gz | tar
xz -C /usr/local/ --strip=1  
RUN \  
apt-get update && \  
apt-get install -y imagemagick  
VOLUME /src  
WORKDIR /src  


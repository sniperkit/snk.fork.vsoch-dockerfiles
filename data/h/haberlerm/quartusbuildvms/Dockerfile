FROM debian:jessie  
MAINTAINER Michael Haberler <haberlerm@gmail.com>  
  
ENV TERM dumb  
  
# apt config: silence warnings and set defaults  
ENV DEBIAN_FRONTEND noninteractive  
ENV DEBCONF_NONINTERACTIVE_SEEN true  
  
# turn off recommends on container OS  
# install required dependencies  
RUN echo 'APT::Install-Recommends "0";\nAPT::Install-Suggests "0";' > \  
/etc/apt/apt.conf.d/01norecommend && \  
apt-get update && \  
apt-get -y install \  
ca-certificates \  
gcc \  
git \  
openjdk-7-jdk \  
libfontconfig1 \  
libglib2.0-0 \  
libsm6 \  
libxrender1 \  
locales \  
make \  
ruby \  
ruby-dev \  
xauth \  
xvfb \  
pkg-config \  
libprotobuf-dev \  
protobuf-compiler \  
python-protobuf \  
python-pip && \  
pip install intelhex && \  
adduser \--disabled-password --gecos '' builder && \  
usermod builder -aG staff && \  
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \  
locale-gen && \  
gem install fpm  
  
  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8  
  
USER builder  


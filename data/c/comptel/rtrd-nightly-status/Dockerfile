FROM alpine:3.7  
  
COPY requirements.txt /tmp/  
  
RUN apk add --no-cache --update \  
autoconf \  
automake \  
bash \  
build-base \  
git \  
file \  
libffi-dev \  
musl \  
openssl-dev \  
python \  
python-dev \  
py-pip \  
nasm \  
&& pip install --requirement /tmp/requirements.txt \  
# Cleanup  
&& rm -rf /tmp/* /var/tmp/*


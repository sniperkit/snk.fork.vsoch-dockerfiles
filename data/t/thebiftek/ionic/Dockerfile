FROM beevelop/cordova:v7.1.0  
MAINTAINER Jakub Pištěk <mail@jakubpistek.cz>  
  
ENV IONIC_VERSION 3.19.1  
COPY licences $ANDROID_HOME  
  
RUN apt-get update && apt-get install -y git bzip2 openssh-client && \  
npm i -g --unsafe-perm ionic@${IONIC_VERSION} && \  
ionic --no-interactive config set -g daemon.updates false && \  
rm -rf /var/lib/apt/lists/* && apt-get clean  
  


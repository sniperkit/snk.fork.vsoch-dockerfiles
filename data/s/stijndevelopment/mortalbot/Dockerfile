# ----------------------------------  
# Pterodactyl NodeJS Dockerfile  
# Environment: Node  
# Minimum Panel Version: 0.6.0  
# ----------------------------------  
FROM ubuntu:16.04  
MAINTAINER Stijn van Nieulande, <stijn.van.nieulande@gmail.com>  
  
ENV DEBIAN_FRONTEND noninteractive  
# Install Dependencies  
RUN dpkg --add-architecture i386 \  
&& apt-get update \  
&& apt-get upgrade -y \  
&& apt-get install -y tar curl gcc g++ lib32gcc1 lib32tinfo5 lib32z1
lib32stdc++6 libtinfo5:i386 libncurses5:i386 libcurl3-gnutls:i386 nodejs npm\  
&& useradd -m -d /home/container container  
  
USER container  
ENV USER container  
ENV HOME /home/container  
  
WORKDIR /home/container  
  
COPY ./entrypoint.sh /entrypoint.sh  
#COPY ./package.json /home/container/package.json  
#COPY ./server.js /home/container/server.js  
RUN wget https://mortalpvp.com/discordbots/MortalBot/package.json -O
/mnt/server/package.json \  
&& wget https://mortalpvp.com/discordbots/MortalBot/server.js -O
/mnt/server/server.js  
  
CMD ["/bin/bash", "/entrypoint.sh"]  


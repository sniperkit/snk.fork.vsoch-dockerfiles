FROM multiarch/debian-debootstrap:armhf-jessie-slim  
  
ARG snapcast_version=0.14.0  
ENV HOST snapserver  
  
RUN apt-get update \  
&& apt-get install -y wget ca-certificates \  
&& rm -rf /var/lib/apt/lists/*  
RUN wget
https://github.com/badaix/snapcast/releases/download/v${snapcast_version}/snapclient_${snapcast_version}_armhf.deb  
RUN dpkg -i snapclient_${snapcast_version}_armhf.deb \  
; apt-get update \  
&& apt-get -f install -y \  
&& rm -rf /var/lib/apt/lists/*  
RUN snapclient -v  
ENTRYPOINT ["/bin/bash","-c","snapclient -h $HOST"]  


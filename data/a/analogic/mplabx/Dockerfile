FROM debian:jessie  
  
MAINTAINER Analogic s.r.o. <info@analogic.cz>  
  
ENV DEBIAN_FRONTEND noninteractive  
  
# Microchip tools require i386 compatability libs  
RUN dpkg \--add-architecture i386 \  
&& apt-get update -yq \  
&& apt-get install -yq \--no-install-recommends curl libc6:i386 \  
libx11-6:i386 libxext6:i386 libstdc++6:i386 libexpat1:i386 \  
libxext6 libxrender1 libxtst6 libgtk2.0-0  
  
# Download and install XC32 compiler  
RUN curl -fSL -A "Mozilla/4.0" -o /tmp/xc32.run
"http://www.microchip.com/mplabxc32linux" \  
&& chmod a+x /tmp/xc32.run \  
&& /tmp/xc32.run \--mode unattended \--unattendedmodeui none \  
\--netservername localhost \--LicenseType FreeMode \  
&& rm /tmp/xc32.run  
ENV PATH /opt/microchip/xc32/v1.40/bin:$PATH  
  
# Download and install MPLAB X IDE  
RUN curl -fSL -A "Mozilla/4.0" -o /tmp/mplabx-installer.tar
"http://www.microchip.com/mplabx-ide-linux-installer" \  
&& tar xf /tmp/mplabx-installer.tar && rm /tmp/mplabx-installer.tar \  
&& USER=root ./*-installer.sh \--nox11 \  
\-- \--unattendedmodeui none \--mode unattended \  
&& rm ./*-installer.sh  
  
# Harmony  
RUN curl -fSL -A "Mozilla/4.0" -o /tmp/harmony.run
"http://www.microchip.com/mymicrochip/filehandler.aspx?ddocname=en584410" \  
&& chmod a+x /tmp/harmony.run \  
&& /tmp/harmony.run \--mode unattended \--unattendedmodeui none \--prefix /opt
&& rm /tmp/harmony.run  
  
VOLUME ["/tmp/.X11-unix"]  
CMD ["/usr/bin/mplab_ide"]  


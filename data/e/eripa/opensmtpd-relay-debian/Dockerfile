FROM debian:jessie  
MAINTAINER Eric Ripa <eric@ripa.io>  
  
RUN echo "deb http://ftp.debian.org/debian jessie-backports main" >>
/etc/apt/sources.list  
RUN apt-get -q update \  
&& apt-get -qy dist-upgrade \  
&& apt-get -qy install opensmtpd \  
&& rm -rf /var/lib/apt/lists/*  
  
RUN echo "docker-smtpd" > /etc/mailname  
RUN mkdir /etc/smtpd  
COPY ./smtpd-template /etc/smtpd-template  
COPY ./entrypoint.sh /  
  
VOLUME ["/etc/smtpd", "/var/vmail"]  
EXPOSE 25 587  
ENTRYPOINT ["/entrypoint.sh"]


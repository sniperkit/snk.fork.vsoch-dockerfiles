FROM alpine  
LABEL maintainer "Andrew Li <andrew.li@hinterlands.tech>"  
  
RUN apk --update add mariadb mariadb-client pwgen && \  
rm -f /var/cache/apk/*  
  
ADD files/run.sh /scripts/run.sh  
RUN mkdir /scripts/pre-exec.d && \  
mkdir /scripts/pre-init.d && \  
chmod -R 755 /scripts  
  
EXPOSE 3306  
VOLUME ["/var/lib/mysql"]  
  
ENTRYPOINT ["/scripts/run.sh"]  


FROM openjdk:8u131-jre-alpine  
  
VOLUME ["/server", "/plugins"]  
WORKDIR /server  
  
ENV BUNGEE_HOME=/server \  
BUNGEE_BASE_URL=https://ci.md-5.net/job/BungeeCord \  
BUNGEE_JAR_URL=https://8-135221131-gh.circle-artifacts.com/0/BungeeCord.jar \  
MEMORY=512m  
  
COPY *.sh /usr/bin/  
  
RUN apk --no-cache add curl bash sudo  
  
EXPOSE 25577  
  
RUN set -x \  
&& addgroup -g 1000 -S bungeecord \  
&& adduser -u 1000 -D -S -G bungeecord bungeecord \  
&& addgroup bungeecord wheel  
  
CMD ["/usr/bin/run-bungeecord.sh"]  


FROM busybox:1.28 AS fetch  
  
ARG VERSION=0.41  
ADD
https://github.com/gohugoio/hugo/releases/download/v${VERSION}/hugo_${VERSION}_Linux-64bit.tar.gz
/hugo.tar.gz  
RUN tar -zxvf hugo.tar.gz  
  
FROM busybox:1.28  
ENV HUGO_BIND="0.0.0.0" \  
HUGO_DESTINATION="/target"  
COPY \--from=fetch /hugo /bin/hugo  
COPY files /  
  
EXPOSE 1313  
VOLUME /src /target  
WORKDIR /src  
  
ENTRYPOINT ["sh", "/run.sh"]  


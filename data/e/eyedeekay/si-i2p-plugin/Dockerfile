FROM alpine:3.6  
RUN apk update -U  
RUN apk add go go-tools go-doc git make musl-dev musl musl-utils  
RUN adduser -s /opt/bin/si-i2p-plugin -S -H -D sii2pplugin sii2pplugin  
COPY . /opt/  
RUN chown -R sii2pplugin /opt/  
WORKDIR /opt/  
RUN make release && chmod a+x /opt/bin/si-i2p-plugin  
RUN apk del go go-tools go-doc git make musl-dev musl musl-utils  
USER sii2pplugin  
CMD /opt/bin/si-i2p-plugin -socks-addr=0.0.0.0 -socks-port=44446 -proxy-
addr=0.0.0.0 -proxy-port=44443 -bridge-addr=sam-host  


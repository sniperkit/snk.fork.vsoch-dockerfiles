FROM alpine:3.6  
  
RUN set -ex \  
&& echo "http://nl.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories
\  
&& apk update \  
&& apk add --no-cache \  
tcpdump \  
bridge-utils \  
netcat-openbsd \  
util-linux \  
iptables \  
iputils \  
iproute2 \  
iftop \  
drill \  
apache2-utils \  
strace \  
curl \  
ethtool \  
ipvsadm \  
ngrep \  
iperf \  
nmap \  
nmap-nping \  
conntrack-tools \  
bind-tools \  
socat  
# apparmor issue #14140  
RUN mv /usr/sbin/tcpdump /usr/bin/tcpdump  
  
ADD netgen.sh /usr/local/bin/netgen  
  
CMD ["sh"]  


FROM amazonlinux:2  
LABEL maintainer="sean@sean.io"  
  
RUN yum -y install curl gnupg2 initscripts iptables iputils lsof nc net-tools
nmap openssl procps strace systemd-sysv tcpdump telnet vim-minimal wget which
ca-certificates  
  
CMD [ "/usr/lib/systemd/systemd" ]  


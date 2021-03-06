# This will generate efrecon/armv7hf-tcl  
FROM efrecon/armv7hf-debian:stretch  
MAINTAINER Emmanuel Frecon <efrecon@gmail.com>  
  
# Set the env variable DEBIAN_FRONTEND to noninteractive to get  
# apt-get working without error output.  
ENV DEBIAN_FRONTEND noninteractive  
  
# Update underlying ubuntu image and all necessary packages.  
RUN [ "cross-build-start" ]  
RUN apt-get update && \  
apt-get upgrade -y && \  
apt-get install -y tcl8.6 \  
tcllib \  
tcl-thread \  
tcl-tclreadline \  
tcl-tls \  
tcl-trf \  
tcl-udp \  
tcl-vfs \  
tclcurl \  
tclgeoip \  
tclodbc \  
tclxml \  
tdom \  
tcl-tclex \  
tcl-memchan \  
tcl-combat \  
tcl-signal \  
tcl-sugar \  
libpgtcl \  
libsqlite-tcl \  
mysqltcl \  
tcl8.6-tdbc \  
tcl8.6-tdbc-mysql \  
tcl8.6-tdbc-odbc \  
tcl8.6-tdbc-postgres \  
tcl8.6-tdbc-sqlite3 \  
itcl3 \  
critcl \  
xotcl \  
tclxapian \  
libtcl-chiark-1  
RUN [ "cross-build-end" ]  
  
# Arrange for a decent tclreadline prompt in interactive mode  
COPY tclshrc /root/.tclshrc  
  
# Export two volumes, one for tcl code and one for data, just in case.  
VOLUME /opt/tcl  
VOLUME /opt/data  
  
# Make sure code put into the special tcl volume can lazily be filled  
# with packages  
ENV TCLLIBPATH /opt/tcl /opt/tcl/lib  
  
# And make the core bare tclsh the entrypoint as the default  
ENTRYPOINT ["tclsh8.6"]


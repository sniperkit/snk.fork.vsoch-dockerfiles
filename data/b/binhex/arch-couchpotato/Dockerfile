FROM binhex/arch-base:20170101-02  
MAINTAINER binhex  
  
# additional files  
##################  
# add supervisor conf file for app  
ADD setup/*.conf /etc/supervisor/conf.d/  
  
# add install and packer bash script  
ADD setup/root/*.sh /root/  
  
# install app  
#############  
# make executable and run bash scripts to install app  
RUN chmod +x /root/*.sh && \  
/bin/bash /root/install.sh  
  
# docker settings  
#################  
# map /config to host defined config path (used to store configuration from
app)  
VOLUME /config  
  
# map /data to host defined data path (used to store downloads or use
blackhole)  
VOLUME /data  
  
# map /media to host defined media path (used to read/write to media library)  
VOLUME /media  
  
# expose port for http  
EXPOSE 5050  
# set permissions  
#################  
# run script to set uid, gid and permissions  
CMD ["/bin/bash", "/root/init.sh"]


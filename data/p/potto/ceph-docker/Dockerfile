# DOCKER-VERSION 1.0.0  
#  
# Ceph Daemon  
#  
# VERSION 0.0.1  
FROM potto/ceph-base:jewel  
MAINTAINER Sébastien Han "seb@redhat.com"  
# patch is only needed until PR #7351 is merged in ceph/ceph (and backported  
# to the version used in ceph/ceph-docker)  
RUN apt-get update && apt-get -y install libjemalloc-dev runit patch sharutils
&& \  
apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*  
  
# Add bootstrap script  
ADD entrypoint.sh /entrypoint.sh  
ADD config.*.sh /  
  
ADD my_init/my_init /sbin/my_init  
RUN chmod +x /sbin/my_init  
RUN mkdir -p /etc/container_environment  
  
ADD 2.sh /etc/runit/2  
RUN chmod +x /etc/runit/2  
  
# ceph defaults key/values for KV store  
ADD ceph.defaults /  
  
# Add templates for confd  
ADD ./confd/templates/* /etc/confd/templates/  
ADD ./confd/conf.d/* /etc/confd/conf.d/  
  
# Patch out ceph-disk, to allow overrides to user and group names  
# This can be removed once PR #7351 is merged in ceph/ceph (and backported to  
# the version used by ceph/ceph-docker)  
#ADD ./ceph-disk.patch /ceph-disk.patch  
#RUN cd /usr/sbin && patch -p1 -n -u < /ceph-disk.patch  
# Preload jemalloc  
ENV LD_PRELOAD /usr/lib/x86_64-linux-gnu/libjemalloc.so  
  
# Add volumes for Ceph config and data  
VOLUME ["/etc/ceph","/var/lib/ceph"]  
  
# Execute the entrypoint  
WORKDIR /  
ENTRYPOINT ["/entrypoint.sh"]  


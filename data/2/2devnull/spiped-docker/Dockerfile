FROM ubuntu:16.04  
RUN apt-get update -y  
  
# Add file & deps  
ADD spiped-1.5.0 /spiped  
RUN apt-get install -y build-essential --fix-missing  
RUN apt-get install -y libssl-dev  
  
# Make Spiped  
RUN cd /spiped && make install  
  
# Add key  
# ADD spiped.key /spiped/spiped.key  
# SSH nonsense  
RUN apt-get install -y openssh-server  
RUN mkdir /root/.ssh  
RUN mkdir /var/run/sshd  
ADD ssh.conf /etc/ssh/sshd_config  
RUN ssh-keygen -q -f /root/.ssh/id_rsa -N ""  
RUN cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys  
RUN chown root:root /root/.ssh/authorized_keys  
  
# RUN /usr/sbin/sshd  
# ssh -oStrictHostKeyChecking=no -f -N -D 0.0.0.0:1080 localhost  
# Aggregate commands into one script  
ADD run.sh /spiped/run.sh  
RUN chmod 777 /spiped/run.sh  
  
ENTRYPOINT ["/spiped/run.sh"]  
  
#RUN /spiped/spiped/spiped -d -s '[0.0.0.0]:8089' -t '[127.0.0.1]:1080' -k
/spiped/spiped.key  
#RUN ls  
# WORKDIR /spiped  
# ENTRYPOINT ["/bin/bash"]  
# CMD ["-d -s '[0.0.0.0]:8022' -t '[127.0.0.1]:1080' -k spiped.key"]  
# local: spiped -e -F -s '[0.0.0.0]:8089' -t '[107.170.94.89]:49159' -k
spiped.key  


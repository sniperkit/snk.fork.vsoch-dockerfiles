FROM ubuntu:xenial-20171006  
  
RUN apt-get update && apt-get install -y python3 python3-pip
python3-setuptools python3-wheel locales && \  
apt-get clean && \  
rm -rf /var/lib/apt/lists/* && \  
locale-gen en_US.UTF-8  
# The /etc/default/locale file, generated by update-locale, is loaded by PAM;  
# see /etc/pam.d/login for example. However, PAM is not invoked when running  
# a command in a Docker container.  
ENV LANG=en_US.UTF-8 \  
LANGUAGE=en_US:en \  
LC_ALL=en_US.UTF-8  


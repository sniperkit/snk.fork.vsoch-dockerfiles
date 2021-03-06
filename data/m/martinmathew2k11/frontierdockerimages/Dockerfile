# © Copyright IBM Corporation 2015.  
#  
# All rights reserved. This program and the accompanying materials  
# are made available under the terms of the Eclipse Public License v1.0  
# which accompanies this distribution, and is available at  
# http://www.eclipse.org/legal/epl-v10.html  
FROM ubuntu:14.04  
ENV TOMCAT_MAJOR_VERSION=8  
ENV TOMCAT_VERSION=8.5.9  
ENV TOMCAT_HOME=/opt/apache-tomcat-$TOMCAT_VERSION  
  
  
MAINTAINER Sam Rogers srogers@uk.ibm.com  
  
LABEL "ProductID"="447aefb5fd1342d5b893f3934dfded73" \  
"ProductName"="IBM Integration Bus" \  
"ProductVersion"="10.0.0.10"  
  
# Install curl  
RUN apt-get update && \  
apt-get install -y curl && \  
rm -rf /var/lib/apt/lists/*  
  
RUN apt-get update \  
&& apt-get install -y wget  
  
RUN apt-get update \  
&& apt-get install -y lsof  
  
# Install IIB V10 Developer edition  
RUN mkdir /opt/ibm && \  
curl
http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/integration/10.0.0.10-IIB-
LINUX64-DEVELOPER.tar.gz \  
| tar zx --exclude iib-10.0.0.10/tools --directory /opt/ibm && \  
/opt/ibm/iib-10.0.0.10/iib make registry global accept license silently  
  
#Install tomcat  
RUN mkdir -p $TOMCAT_HOME && cd /opt && \  
wget
https://archive.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR_VERSION/v$TOMCAT_VERSION/bin/apache-
tomcat-$TOMCAT_VERSION.tar.gz && \  
tar -xvf apache-tomcat-$TOMCAT_VERSION.tar.gz && rm -f apache-
tomcat-$TOMCAT_VERSION.tar.gz  
  
# Add image configuration and scripts  
ADD start-tomcat.sh /start-tomcat.sh  
RUN chmod 755 /*.sh  
ADD supervisord-tomcat.conf /etc/supervisor/conf.d/supervisord-tomcat.conf  
  
# Configure system  
COPY kernel_settings.sh /tmp/  
RUN echo "IIB_10:" > /etc/debian_chroot && \  
touch /var/log/syslog && \  
chown syslog:adm /var/log/syslog && \  
chmod +x /tmp/kernel_settings.sh;sync && \  
/tmp/kernel_settings.sh  
  
# Create user to run as  
RUN useradd --create-home --home-dir /home/iibuser -G mqbrkrs,sudo iibuser &&
\  
sed -e 's/^%sudo .*/%sudo ALL=NOPASSWD:ALL/g' -i /etc/sudoers  
  
# Increase security  
RUN sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password && \  
sed -i 's/PASS_MIN_DAYS\t0/PASS_MIN_DAYS\t1/' /etc/login.defs && \  
sed -i 's/PASS_MAX_DAYS\t99999/PASS_MAX_DAYS\t90/' /etc/login.defs  
  
# Copy in script files  
COPY iib_manage.sh /usr/local/bin/  
COPY iib_set_properties.sh /usr/local/bin/  
COPY start-tomcat.sh /usr/local/bin/  
COPY iib-license-check.sh /usr/local/bin/  
COPY iib_env.sh /usr/local/bin/  
RUN chmod +rx /usr/local/bin/*.sh  
  
# Set BASH_ENV to source mqsiprofile when using docker exec bash -c  
ENV BASH_ENV=/usr/local/bin/iib_env.sh  
ENV MQSI_MQTT_LOCAL_HOSTNAME=127.0.0.1  
RUN chmod 755 /*.sh  
WORKDIR $TOMCAT_HOME  
RUN chmod +rx $TOMCAT_HOME/bin/*.sh  
RUN chmod -R 777 $TOMCAT_HOME/*  
ADD CrossConnectSPA $TOMCAT_HOME/webapps/CrossConnectSPA  
ADD bars /bars  
  
# Expose default admin port and http port  
EXPOSE 4414 7800 8080  
USER iibuser  
  
# Set entrypoint to run management script  
ENTRYPOINT ["iib_manage.sh"]  
  


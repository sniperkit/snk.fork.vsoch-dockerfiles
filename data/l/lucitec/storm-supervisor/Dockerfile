FROM lucitec/storm  
MAINTAINER Lucitec  
  
EXPOSE 6700  
EXPOSE 6701  
EXPOSE 6702  
EXPOSE 6703  
EXPOSE 8000  
RUN /usr/bin/config-supervisord.sh supervisor  
RUN /usr/bin/config-supervisord.sh logviewer  
ADD start-supervisor.sh /usr/bin/start-supervisor.sh  
CMD /usr/bin/start-supervisor.sh  
  


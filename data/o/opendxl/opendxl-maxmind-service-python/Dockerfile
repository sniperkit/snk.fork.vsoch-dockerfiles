# Base image from Python 2.7 (slim)  
FROM python:2.7-slim  
  
VOLUME ["/opt/dxlmaxmindservice-config"]  
  
# Build service  
COPY . /tmp/build  
WORKDIR /tmp/build  
  
# Install application package and its dependencies  
RUN pip install .  
  
# Cleanup build  
RUN rm -rf /tmp/build  
  
################### INSTALLATION END #######################  
#  
# Run the application.  
#  
# NOTE: The configuration files for the application must be  
# mapped to the path: /opt/dxlmaxmindservice-config  
#  
# For example, specify a "-v" argument to the run command  
# to mount a directory on the host as a data volume:  
#  
# -v /host/dir/to/config:/opt/dxlmaxmindservice-config  
#  
CMD ["python", "-m", "dxlmaxmindservice", "/opt/dxlmaxmindservice-config"]  


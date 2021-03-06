FROM reecerobinson/spark-nosql:1.6.0  
MAINTAINER docker@reecerobinson.co.nz  
  
ENV APACHE_SPARK_VERSION=1.6.0  
ENV SPARK_HOME /usr/local/spark  
ENV PATH $PATH:$SPARK_HOME/bin  
  
RUN apt-get update && \  
apt-get install -y libc6 libc6-dev libc-dev udev libncurses5-dev && \  
apt-get install -y python-dev && \  
apt-get install -y python-pip && \  
apt-get install -y libfreetype6 libfreetype6-dev zlib1g-dev && \  
apt-get install -y python-matplotlib && \  
apt-get install -y python-sklearn && \  
apt-get install -y libopenblas-dev && \  
apt-get install -y python-scipy && \  
apt-get clean  
  
RUN pip install py4j && \  
pip install numpy && \  
pip install redis && \  
pip install pandas && \  
pip install bokeh && \  
pip install --upgrade scikit-learn && \  
pip install "ipython[All]" && \  
pip install cassandra-driver  
  
COPY ipython /root/.ipython  
COPY kernels /root/.ipython/kernels  
COPY test_helper /tmp  
RUN cd /tmp && python setup.py install && rm -rf /tmp/*  
  
RUN mkdir /notebooks  
VOLUME ["/notebooks"]  
EXPOSE 8888 4040  
# update boot script  
COPY bootstrap.sh /etc/bootstrap.sh  
RUN chown root.root /etc/bootstrap.sh  
RUN chmod 700 /etc/bootstrap.sh  
  
COPY hive-site.xml /usr/local/spark/conf/hive-site.xml  
COPY lib/* /usr/local/spark/lib/  
  
ENV SPARK_EXECUTOR_MEMORY 512m  
ENV SPARK_DRIVER_MEMORY 1g  
  
ENTRYPOINT ["/etc/bootstrap.sh"]  


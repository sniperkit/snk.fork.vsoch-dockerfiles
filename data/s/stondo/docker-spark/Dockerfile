FROM progrium/busybox  
RUN opkg-install curl ca-certificates bash gzip zlib  
  
# JAVA  
ENV JAVA_HOME /usr/jdk1.7.0_80  
ENV PATH $PATH:$JAVA_HOME/bin  
RUN curl -sL --retry 3 --insecure \  
\--header "Cookie: oraclelicense=accept-securebackup-cookie;" \  
"http://download.oracle.com/otn-
pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.tar.gz" \  
| gunzip \  
| tar x -C /usr/ \  
&& ln -s $JAVA_HOME /usr/java \  
&& rm -rf $JAVA_HOME/man  
  
# SPARK  
ENV SPARK_VERSION 1.3.1  
ENV HADOOP_VERSION 2.4  
ENV SPARK_PACKAGE $SPARK_VERSION-bin-hadoop$HADOOP_VERSION  
ENV SPARK_HOME /usr/spark-$SPARK_PACKAGE  
ENV PATH $PATH:$SPARK_HOME/bin  
RUN curl -sL --retry 3 \  
"http://mirrors.ibiblio.org/apache/spark/spark-$SPARK_VERSION/spark-$SPARK_PACKAGE.tgz"
\  
| gunzip \  
| tar x -C /usr/ \  
&& ln -s $SPARK_HOME /usr/spark  
  
CMD /usr/spark/bin/spark-class org.apache.spark.deploy.master.Master  


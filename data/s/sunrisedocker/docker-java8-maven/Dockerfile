# Ubuntu 16.04  
# Oracle Java 1.8.0_101 64 bit  
# Maven 3.3.9  
FROM ubuntu:16.04  
MAINTAINER Yevhen Strazhko (https://github.com/strazhkoe)  
  
# this is a non-interactive automated build - avoid some warning messages  
ENV DEBIAN_FRONTEND noninteractive  
  
# update dpkg repositories  
RUN apt-get update  
  
# install wget  
RUN apt-get install -y wget  
  
# get maven 3.3.9  
RUN wget --no-verbose -O /tmp/apache-maven-3.3.9.tar.gz
http://archive.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-
maven-3.3.9-bin.tar.gz  
  
# verify checksum  
RUN echo "516923b3955b6035ba6b0a5b031fbd8b /tmp/apache-maven-3.3.9.tar.gz" |
md5sum -c  
  
# install maven  
RUN tar xzf /tmp/apache-maven-3.3.9.tar.gz -C /opt/  
RUN ln -s /opt/apache-maven-3.3.9 /opt/maven  
RUN ln -s /opt/maven/bin/mvn /usr/local/bin  
RUN rm -f /tmp/apache-maven-3.3.9.tar.gz  
ENV MAVEN_HOME /opt/maven  
  
# remove download archive files  
RUN apt-get clean  
  
# set shell variables for java installation  
ENV java_version 1.8.0_131  
ENV filename jdk-8u131-linux-x64.tar.gz  
  
# download java, accepting the license agreement  
RUN wget --no-cookies --no-check-certificate --header "Cookie:
gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-
cookie" "http://download.oracle.com/otn-
pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz"  
  
# unpack java  
RUN mkdir /opt/java-oracle && tar -zxf $filename -C /opt/java-oracle/  
ENV JAVA_HOME /opt/java-oracle/jdk$java_version  
ENV PATH $JAVA_HOME/bin:$PATH  
  
# configure symbolic links for the java and javac executables  
RUN update-alternatives --install /usr/bin/java java $JAVA_HOME/bin/java 20000
&& update-alternatives --install /usr/bin/javac javac $JAVA_HOME/bin/javac
20000  
  
# install git  
RUN apt-get install -y git  
  
CMD [""]  
  


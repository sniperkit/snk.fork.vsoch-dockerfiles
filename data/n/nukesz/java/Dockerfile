FROM ubuntu:latest  
MAINTAINER Norbert Benczur, norbert.benczur@gmail.com  
RUN apt-get update \  
&& apt-get install -y software-properties-common \  
&& add-apt-repository -y ppa:webupd8team/java \  
&& apt-get update \  
&& echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true
| /usr/bin/debconf-set-selections \  
&& apt-get install -y oracle-java8-installer \  
&& apt-get install unzip \  
&& apt-get clean  


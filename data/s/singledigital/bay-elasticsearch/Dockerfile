FROM amazeeio/elasticsearch  
  
WORKDIR /tmp  
  
RUN yum install unzip -y \  
&& yum install java-1.8.0-openjdk-devel -y \  
&& wget https://github.com/sscarduzio/elasticsearch-readonlyrest-
plugin/archive/v1.16.21-pre1_es6.1.1.zip \  
&& unzip v1.16.21-pre1_es6.1.1.zip \  
&& mv elasticsearch-readonlyrest-plugin-1.16.21-pre1_es6.1.1 readonlyrest  
  
COPY readonlyrest/build.gradle /tmp/readonlyrest/core/build.gradle  
COPY readonlyrest/gradle.properties /tmp/readonlyrest/es61x/gradle.properties  
COPY readonlyrest/settings.gradle tmp/readonlyrest/settings.gradle  
  
WORKDIR /tmp/readonlyrest  
  
RUN ./gradlew --stacktrace es61x:ror '-PesVersion=6.1.1'  
  
WORKDIR /usr/share/elasticsearch  
  
COPY readonlyrest/readonlyrest.yml config/readonlyrest.yml  
  
RUN bin/elasticsearch-plugin install
file:///tmp/readonlyrest/es61x/build/distributions/readonlyrest-1.16.21-pre1_es6.1.1.zip
--batch \  
&& chown elasticsearch:root config/readonlyrest.yml \  
&& chown -R elasticsearch:root plugins/readonlyrest \  
&& yum erase java-1.8.0-openjdk-devel -y \  
&& yum erase unzip -y\  
&& rm -rf /tmp/v1.16.21-pre1_es6.1.1.zip \  
&& rm -rf /tmp/readonlyrest  
  


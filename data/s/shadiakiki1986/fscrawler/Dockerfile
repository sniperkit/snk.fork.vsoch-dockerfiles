# It suffices to use `FROM openjdk:alpine` to use alpine 3.4, but tesseract is
included in apk in alpine 3.5  
# so copying the openjdk:alpine dockerfile, but using alpine:3.5  
# Ref: https://github.com/docker-
library/openjdk/blob/0476812eabd178c77534f3c03bd0a2673822d7b9/8-jdk/alpine/Dockerfile  
# https://pkgs.alpinelinux.org/packages?name=tesseract-
ocr&branch=&repo=&arch=&maintainer=  
#  
###############################################################  
# Edit 2018-01-08  
# Updating FROM based on above, and no need for special installations  
#  
# FROM alpine:3.5  
#  
# # A few problems with compiling Java from source:  
# # 1. Oracle. Licensing prevents us from redistributing the official JDK.  
# # 2. Compiling OpenJDK also requires the JDK to be installed, and it gets  
# # really hairy.  
#  
# # Default to UTF-8 file.encoding  
# ENV LANG C.UTF-8  
#  
# # add a simple script that can auto-detect the appropriate JAVA_HOME value  
# # based on whether the JDK or only the JRE is installed  
# RUN { \  
# echo '#!/bin/sh'; \  
# echo 'set -e'; \  
# echo; \  
# echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"';
\  
# } > /usr/local/bin/docker-java-home \  
# && chmod +x /usr/local/bin/docker-java-home  
# ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk  
# ENV PATH
$PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin  
#  
# #ENV JAVA_VERSION 8u111  
# #ENV JAVA_ALPINE_VERSION 8.111.14-r0  
# # changed the below openjdk from constraint:  
# # openjdk8 # ="$JAVA_ALPINE_VERSION" \  
# # so that I don't need to worry about updating this with every repo update
of alpine 3.5  
# # Also commented out the unused env vars above  
#  
# RUN set -x \  
# && apk add --update --no-cache \  
# openjdk8 \  
# && [ "$JAVA_HOME" = "$(docker-java-home)" ]  
###############################################################  
  
FROM openjdk:8-jdk-alpine3.7  
  
############################################################  
# Now that alpine >= 3.5 is installed, benefit by installing tesseract  
RUN set -x && apk --update --no-cache add tesseract-ocr  
# Download training data  
# https://github.com/tesseract-ocr/tesseract/wiki#linux  
# I would have expected to need to download 3.04 traineddata  
# https://github.com/tesseract-ocr/tessdata/raw/3.04.00/eng.traineddata  
# but this seems to be working with the master version  
RUN set -x && apk --update --no-cache add openssl  
RUN wget https://github.com/tesseract-ocr/tessdata/raw/master/eng.traineddata
-O /usr/share/tessdata/eng.traineddata  
  
#####################################################  
# Rest of file shamelessly copied (more or less) from the elasticsearch:alpine
dockerfile  
# Ref: https://github.com/docker-
library/elasticsearch/blob/f2e19796b765e2e448d0e8c651d51be992b56d08/5/alpine/Dockerfile  
  
# ensure fscrawler user exists  
RUN addgroup -S fscrawler && adduser -S -G fscrawler fscrawler  
  
# grab su-exec for easy step-down from root  
# and bash for "bin/fscrawler" among others  
RUN apk add --no-cache 'su-exec>=0.2' bash openssl  
  
# for versions released on Maven Central  
# WORKDIR /usr/share/fscrawler  
# ENV FSCRAWLER_VERSION 2.4  
# ENV
FSCRAWLER_ZIP="https://repo1.maven.org/maven2/fr/pilato/elasticsearch/crawler/fscrawler/2.4/fscrawler-2.4.zip"  
# RUN wget -O fscrawler.zip "$FSCRAWLER_ZIP";  
  
# for versions snapshotted on sonatype  
# WORKDIR /usr/share/fscrawler  
# ENV FSCRAWLER_VERSION 2.5-SNAPSHOT  
# ENV
FSCRAWLER_ZIP="https://oss.sonatype.org/content/repositories/snapshots/fr/pilato/elasticsearch/crawler/fscrawler/2.5-SNAPSHOT/fscrawler-2.5-20171027.095913-3.zip"  
# RUN wget -O fscrawler.zip "$FSCRAWLER_ZIP";  
  
# for compiling from source  
WORKDIR /usr/src/  
ENV FSCRAWLER_VERSION 2.5-SNAPSHOT  
RUN apk add --update --no-cache maven wget # openjdk8 git  
RUN wget https://github.com/dadoonet/fscrawler/archive/master.zip && unzip
master.zip  
WORKDIR /usr/src/fscrawler-master  
ENV MAVEN_OPTS="-Xmx2g -Xms2g"  
# -XX:MaxPermSize=2g  
RUN java -d64 -version # check if 64-bit available  
RUN java -version # check if 32-bit available  
RUN mvn compile  
RUN mvn clean install -X -DskipTests \  
&& cp target/fscrawler-$FSCRAWLER_VERSION.zip
/usr/share/fscrawler/fscrawler.zip  
  
# continue  
WORKDIR /usr/share/fscrawler  
ENV PATH /usr/share/fscrawler/bin:$PATH  
  
# Remove logs path from below as it was just copy-pasted from elasticsearch  
# ./logs \  
RUN set -ex; \  
\  
unzip fscrawler.zip; \  
rm fscrawler.zip; \  
\  
for path in \  
./data \  
./config \  
; do \  
mkdir -p "$path"; \  
chown -R fscrawler:fscrawler "$path"; \  
done;  
  
RUN mv fscrawler-$FSCRAWLER_VERSION/* .; \  
rmdir fscrawler-$FSCRAWLER_VERSION;  
#RUN chown -R fscrawler:fscrawler .  
#USER fscrawler  
  
VOLUME /usr/share/fscrawler/data  
RUN mkdir /usr/share/fscrawler/config-mount \  
&& touch /usr/share/fscrawler/config-mount/empty  
  
COPY entry.sh /  
  
ENTRYPOINT ["/entry.sh"]  
CMD ["fscrawler", "--trace", "--config_dir", "/usr/share/fscrawler/config",
"myjob"]  


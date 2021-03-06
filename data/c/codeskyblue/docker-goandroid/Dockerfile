# Dockerfile to build an image with the local version of go.mobile.  
#  
# > docker build -t mobile /path/to/go.mobile  
# > docker run --rm mobile /bin/bash -c 'cd example/basic && ./make.bash'  
FROM ubuntu:12.04  
# Install system-level dependencies.  
ENV DEBIAN_FRONTEND noninteractive  
RUN echo "debconf shared/accepted-oracle-license-v1-1 select true" | debconf-
set-selections && \  
echo "debconf shared/accepted-oracle-license-v1-1 seen true" | debconf-set-
selections  
RUN apt-get update && \  
apt-get -y install build-essential python-software-properties bzip2 curl \  
git subversion mercurial bzr \  
libncurses5:i386 libstdc++6:i386 zlib1g:i386 && \  
add-apt-repository ppa:webupd8team/java && \  
apt-get update && \  
apt-get -y install oracle-java6-installer  
  
# Install Ant.  
RUN curl -L http://archive.apache.org/dist/ant/binaries/apache-
ant-1.9.2-bin.tar.gz | tar xz -C /usr/local  
ENV ANT_HOME /usr/local/apache-ant-1.9.2  
# Install Android SDK.  
RUN curl -L http://dl.google.com/android/android-sdk_r23.0.2-linux.tgz | tar
xz -C /usr/local  
ENV ANDROID_HOME /usr/local/android-sdk-linux  
RUN echo y | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter
build-tools-19.1.0 && \  
echo y | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter
platform-tools && \  
echo y | $ANDROID_HOME/tools/android update sdk --no-ui --all --filter
android-19  
  
# Install Android NDK.  
RUN curl -L http://dl.google.com/android/ndk/android-
ndk-r9d-linux-x86_64.tar.bz2 | tar xj -C /usr/local  
ENV NDK_ROOT /usr/local/android-ndk-r9d  
RUN $NDK_ROOT/build/tools/make-standalone-toolchain.sh --platform=android-9
--install-dir=$NDK_ROOT \--system=linux-x86_64  
  
# Update PATH for the above.  
ENV PATH $PATH:$ANDROID_HOME/tools  
ENV PATH $PATH:$ANDROID_HOME/platform-tools  
ENV PATH $PATH:$NDK_ROOT  
ENV PATH $PATH:$ANT_HOME/bin  
  
# Install Go.  
ENV GOROOT /go  
ENV GOPATH /gopath  
ENV PATH $PATH:$GOROOT/bin:$GOPATH/bin  
RUN curl https://go.googlecode.com/archive/default.tar.gz | tar xz -C / && \  
mv /go-default $GOROOT && \  
echo devel > $GOROOT/VERSION && \  
cd $GOROOT/src && \  
./all.bash && \  
CC_FOR_TARGET=$NDK_ROOT/bin/arm-linux-androideabi-gcc GOOS=android GOARCH=arm
GOARM=7 ./make.bash  
  
# Copy the local version of go.mobile to GOPATH.  
ADD . /gopath/src/golang.org/x/mobile  
  
# Install dependencies. This will not overwrite the local copy.  
RUN go get -d -t golang.org/x/mobile/...  
RUN go get code.google.com/p/go.mobile/cmd/gobind  
  
# -- bellow modified by codeskyblue --  
ENV HOME /root  
RUN apt-get install -y unzip vim tmux htop tree gettext ack-grep  
  
# Install gradle  
# http://www.gradle.org/downloads  
#  
ENV GRADLE_VERSION 1.12  
ENV GRADLE_ROOT /usr/local/gradle-$GRADLE_VERSION  
RUN curl -L -O
https://services.gradle.org/distributions/gradle-$GRADLE_VERSION-bin.zip &&
unzip gradle-$GRADLE_VERSION-bin.zip -d /usr/local  
ENV PATH $PATH:$GRADLE_ROOT/bin  
# run gradle  
# Install syncthing  
ENV STHOME /root/.config/syncthing  
RUN mkdir -p $STHOME  
RUN curl -L http://goandroid.qiniudn.com/syncthing-linux-amd64-v0.10.6.tar.gz
| tar xz -C /usr/local  
RUN ln -sf /usr/local/syncthing-linux-amd64-v0.10.6/syncthing /usr/bin/  
RUN syncthing -generate=$STHOME  
RUN sed -i 's/127.0.0.1:[0-9]*/0.0.0.0:8080/g' $STHOME/config.xml  
  
WORKDIR /gopath/src/golang.org/x/mobile  
CMD ["bash"]  


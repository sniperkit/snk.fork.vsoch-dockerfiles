# Cloud9 server for glang dev  
# A lot inspired by https://hub.docker.com/_/golang/ (near similar instruction
but based on cloud9, GOPATH /workspace)  
FROM sapk/cloud9  
MAINTAINER Antoine GIRARD <antoine.girard@sapk.fr>  
  
# gcc for cgo  
RUN apt-get update && apt-get install -y --no-install-recommends \  
g++ \  
gcc \  
libc6-dev \  
make \  
&& rm -rf /var/lib/apt/lists/*  
  
ENV GOLANG_VERSION 1.9.2  
ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-
amd64.tar.gz  
ENV GOLANG_DOWNLOAD_SHA256
de874549d9a8d8d8062be05808509c09a88a248e77ec14eb77453530829ac02b  
  
ENV GOPATH /workspace  
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH  
  
RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \  
&& echo "$GOLANG_DOWNLOAD_SHA256 golang.tar.gz" | sha256sum -c - \  
&& tar -C /usr/local -xzf golang.tar.gz \  
&& echo 'export PATH=$GOPATH/bin:/usr/local/go/bin:$PATH' >> /root/.bashrc \  
&& rm golang.tar.gz  


FROM debian:wheezy  
MAINTAINER yigal@publysher.nl  
  
# Install pygments (for syntax highlighting)  
RUN apt-get -qq update \  
&& DEBIAN_FRONTEND=noninteractive apt-get -qq install -y --no-install-
recommends python-pygments git-core \  
&& rm -rf /var/lib/apt/lists/*  
  
# Download and install hugo  
ENV HUGO_VERSION 0.14  
ENV HUGO_BINARY hugo_${HUGO_VERSION}_linux_amd64  
  
ADD
https://github.com/spf13/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY}.tar.gz
/usr/local/  
RUN tar xzf /usr/local/${HUGO_BINARY}.tar.gz -C /usr/local/ \  
&& ln -s /usr/local/${HUGO_BINARY}/${HUGO_BINARY} /usr/local/bin/hugo \  
&& rm /usr/local/${HUGO_BINARY}.tar.gz  
  
# Create working directory  
RUN mkdir /usr/share/blog  
WORKDIR /usr/share/blog  
  
# Expose default hugo port  
EXPOSE 1313  
# Automatically build site  
ONBUILD ADD site/ /usr/share/blog  
ONBUILD RUN hugo -d /usr/share/nginx/html/  
  
# By default, serve site  
ENV HUGO_BASE_URL http://localhost:1313  
CMD hugo server -b ${HUGO_BASE_URL}  


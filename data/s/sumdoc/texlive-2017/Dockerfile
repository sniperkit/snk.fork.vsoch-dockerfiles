FROM debian:sid  
#FROM ubuntu  
ENV LANG=C.UTF-8 \  
LC_ALL=C.UTF-8  
ARG BUILD_DATE  
  
LABEL org.label-schema.build-date=$BUILD_DATE \  
maintainer="Dr Suman Khanal <suman81765@gmail.com>"  
RUN apt-get update \  
&& apt-get install -y gnupg wget curl libgetopt-long-descriptive-perl \  
libdigest-perl-md5-perl python-pygments fontconfig && rm -rf
/var/lib/apt/lists/*  
  
WORKDIR /  
RUN curl -sL http://mirror.utexas.edu/ctan/systems/texlive/tlnet/install-tl-
unx.tar.gz | tar zxf - \  
&& mv install-tl-20* install-tl \  
&& cd install-tl \  
&& echo "selected_scheme scheme-full" > profile \  
&& ./install-tl -repository
http://mirror.utexas.edu/ctan/systems/texlive/tlnet -profile profile \  
&& cd .. \  
&& rm -rf install-tl  
  
  
#curl -sL http://mirror.utexas.edu/ctan/systems/texlive/tlnet/install-tl-
unx.tar.gz | tar zxf -  
ENV PATH /usr/local/texlive/2017/bin/x86_64-linux:$PATH  
WORKDIR /home  
  
CMD ["tlmgr", "--version"]  


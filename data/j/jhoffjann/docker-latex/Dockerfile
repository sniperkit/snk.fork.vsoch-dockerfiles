# Copyright 2016 Jannik Hoffjann All rights reserved.  
#  
# Licensed under the Apache License, Version 2.0 (the "License");  
# you may not use this file except in compliance with the License.  
# You may obtain a copy of the License at  
#  
# http://www.apache.org/licenses/LICENSE-2.0  
#  
# Unless required by applicable law or agreed to in writing, software  
# distributed under the License is distributed on an "AS IS" BASIS,  
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  
# See the License for the specific language governing permissions and  
# limitations under the License.  
FROM phusion/baseimage:latest  
MAINTAINER Jannik Hoffjann<github@jannikhoffjann.de>  
  
ENV DEBIAN_FRONTEND noninteractive  
RUN apt-get --yes update && \  
apt-get --yes dist-upgrade  
RUN apt-get --yes --no-install-recommends --no-install-suggests install
texlive-full  
RUN apt-get --yes install fonts-linuxlibertine fonts-inconsolata fonts-oflb-
asana-math && \  
apt-get clean && \  
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*  
  
VOLUME /src  
WORKDIR /src  


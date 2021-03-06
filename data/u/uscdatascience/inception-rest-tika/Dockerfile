#  
# Licensed to the Apache Software Foundation (ASF) under one or more  
# contributor license agreements. See the NOTICE file distributed with  
# this work for additional information regarding copyright ownership.  
# The ASF licenses this file to You under the Apache License, Version 2.0  
# (the "License"); you may not use this file except in compliance with  
# the License. You may obtain a copy of the License at  
#  
# http://www.apache.org/licenses/LICENSE-2.0  
#  
# Unless required by applicable law or agreed to in writing, software  
# distributed under the License is distributed on an "AS IS" BASIS,  
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  
# See the License for the specific language governing permissions and  
# limitations under the License.  
#  
FROM uscdatascience/tf-tika-base  
MAINTAINER Apache Tika Team  
  
# Download the pretrained inception v4 checkpoint & other meta files  
WORKDIR /usr/share/apache-tika/models/dl/image-video/recognition/  
  
RUN curl -O
http://download.tensorflow.org/models/inception_v4_2016_09_09.tar.gz && \  
tar -xzvf inception_v4_2016_09_09.tar.gz && rm -rf
inception_v4_2016_09_09.tar.gz && \  
curl -O
https://raw.githubusercontent.com/tensorflow/models/master/research/inception/inception/data/imagenet_lsvrc_2015_synsets.txt
&& \  
curl -O
https://raw.githubusercontent.com/tensorflow/models/master/research/inception/inception/data/imagenet_metadata.txt  
  
# Download server related source files  
WORKDIR /usr/share/apache-tika/src/dl/image-video/recognition/  
  
RUN curl -O https://raw.githubusercontent.com/ThejanW/tika/master/tika-
parsers/src/main/resources/org/apache/tika/parser/recognition/tf/inceptionapi.py
&& \  
curl -O https://raw.githubusercontent.com/ThejanW/tika/master/tika-
parsers/src/main/resources/org/apache/tika/parser/recognition/tf/inception_v4.py
&& \  
chmod +x inceptionapi.py  
  
WORKDIR /  
  
# Remove unnecessary packages comes with base  
RUN apt-get -y remove \  
apt-utils \  
curl \  
git \  
software-properties-common \  
unzip \  
wget  
  
# Add symbolic link to inceptionapi.py  
RUN ln -s /usr/share/apache-tika/src/dl/image-
video/recognition/inceptionapi.py /usr/bin/inceptionapi  
  
# Expose API port, this is the default port  
EXPOSE 8764  
CMD inceptionapi


################################################################################  
# #  
# {o,o} #  
# |)__) #  
# -"-"\- #  
# #  
################################################################################  
#  
#  
#  
#################################---FROM---#####################################  
  
FROM ubuntu  
  
################################################################################  
  
#################################---INFO---#####################################  
  
MAINTAINER Lovrenc Avsenek <a.lovrenc@gmail.com>  
  
################################################################################  
  
#################################---ENV---######################################  
  
ENV export DEBIAN_FRONTEND noninteractive  
  
################################################################################  
  
################################---BUILD---#####################################  
  
RUN apt-get update && \  
apt-get upgrade --yes && \  
apt-get install --yes \  
curl \  
git \  
npm && \  
apt-get clean  
  
RUN ln -s /usr/bin/nodejs /usr/bin/node; \  
curl -sL https://deb.nodesource.com/setup_6.x | bash -; \  
apt-get install -y -q \  
nodejs; \  
npm install -g npm  
RUN npm install --global \  
foundation-cli \  
grunt \  
grunt-cli \  
gulp \  
bower \  
webpack \  
load-grunt-tasks \  
time-grunt \  
yarn  
  
################################################################################  


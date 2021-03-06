FROM debian:stable  
  
MAINTAINER David Pose <dpose@bitergia.com>, Alvaro del Castillo
<acs@bitergia.com>, David Muriel <dmuriel@bitergia.com>  
  
ENV KIBANA_BRANCH integration-4.4.1-public  
  
# add our user and group first to make sure their IDs get assigned
consistently  
RUN groupadd --system kibana && \  
useradd --system \--create-home --gid kibana kibana  
  
RUN apt-get update && \  
apt-get install -y \--no-install-recommends \  
ca-certificates \  
curl \  
wget \  
git \  
python \  
zip \  
make \  
g++ \  
&& \  
  
# install nvm  
curl -o /tmp/nvm-install.sh -sSL
https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh && \  
chmod +x /tmp/nvm-install.sh && \  
bash -c /tmp/nvm-install.sh && \  
. /root/.bashrc && \  
  
# install kibiter  
git clone --depth 1 \--branch ${KIBANA_BRANCH}
https://github.com/grimoirelab/kibiter.git && \  
cd kibiter && \  
bash -c "echo 4.3.0 > .node-version" && \  
nvm install "$( cat .node-version )" && \  
npm install -g grunt-cli && \  
npm install && \  
sed -e "/return \\[/,/\\].map/ {/\\(windows\|darwin\|x86\\)/ d;s/,//g}" -i
tasks/config/platforms.js && \  
grunt --no-color _build:downloadNodeBuilds:start
_build:downloadNodeBuilds:finish && \  
grunt --no-color build && \  
tar xfz target/kibana-*-linux-x64.tar.gz --directory /opt && \  
mv /opt/kibana-*-linux-x64* /opt/kibana && \  
cd /root && \  
chown -R kibana:kibana /opt/kibana && \  
  
# cleanup to thin the final image  
rm -rf /kibiter && \  
apt-get purge -y g++ make python git zip wget && \  
apt-get autoremove --purge -y && \  
apt-get clean && \  
find /var/lib/apt/lists -type f -delete && \  
rm -rf /root/.npm /root/.node-gyp /root/.nvm /tmp/* /var/tmp/* && \  
bash -c 'find /usr/share/locale -maxdepth 1 -mindepth 1 -type d | grep -v -e
"en_US" | xargs rm -rfv' && \  
bash -c 'localedef --list-archive | grep -v -e "en_US" | xargs localedef
--delete-from-archive' && \  
rm -rf /usr/share/doc/*  
  
# grab gosu for easy step-down from root  
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys
B42F6819007F00F88E364FD4036A9C25BF357DD4  
RUN arch="$(dpkg --print-architecture)" \  
&& set -x \  
&& curl -o /usr/local/bin/gosu -sSL
"https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch" \  
&& curl -o /usr/local/bin/gosu.asc -sSL
"https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch.asc" \  
&& gpg --verify /usr/local/bin/gosu.asc \  
&& rm /usr/local/bin/gosu.asc \  
&& chmod +x /usr/local/bin/gosu  
  
ENV PATH /opt/kibana/bin:$PATH  
  
# Remove kibana optimize so it is regenerated when we modify src files in
containers  
RUN rm -rf /opt/kibana/optimize/*  
  
COPY ./docker-entrypoint.sh /  
  
EXPOSE 5601  
ENTRYPOINT ["/docker-entrypoint.sh"]  
CMD ["kibana"]  


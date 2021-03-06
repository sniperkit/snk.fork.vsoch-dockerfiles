#  
# This is GCE installer image for OpenShift.  
#  
# It expects to have the following files mounted:  
# - required:  
# /usr/local/install/data/config.sh - a shell script that will be sourced for
config (see config.sh.example)  
# /usr/local/install/data/gce.json - the service account data for GCE in JSON
format  
# /usr/local/install/data/gce.pem - the service account data for GCE in PEM
format  
# - optional:  
# /usr/local/install/data/ssh-publickey - the public key for SSH to GCE nodes,
will be generated  
# /usr/local/install/data/ssh-privatekey - the private key for SSH to GCE
nodes, will be generated  
# /usr/local/install/data/ssl.pem - master serving certificate, signed for
MASTER_DNS_NAME  
# /usr/local/install/data/ssl.key - the private key for the master serving
certificate  
#  
# It must have the following env vars:  
# - optional:  
# GCE_PEM_FILE_PATH: path to a GCE PEM service account key, mounted in  
# INVENTORY_IP_TYPE: *external*|internal  
# CONFIG_SCRIPT: path to script for configuration  
# STARTUP_INSTANCE_DATA_PATH: path to a directory containing instance data to
upload  
#  
# The standard name for this image is openshift/install-gce  
#  
FROM openshift/origin-base  
  
LABEL io.k8s.display-name="OpenShift GCE Install Environment" \  
io.k8s.description="This image helps install OpenShift onto GCE."  
  
ENV HOME=/usr/local/install \  
GOOGLE_CLOUD_SDK_VERSION=130.0.0 \  
OPENSHIFT_ANSIBLE_TAG=release-1.3 \  
GCE_PEM_FILE_PATH=/usr/local/install/data/gce.pem \  
CONFIG_SCRIPT=/usr/local/install/data/config.sh  
  
# package atomic-openshift-utils missing  
RUN mkdir -p $HOME/{bin,data,instance-data,gce-cli/working} && \  
mkdir -p /home/cloud-user/.ssh && \  
chmod uga+rwx -R $HOME /home/cloud-user && \  
ln -s $HOME/data/ssh-privatekey /home/cloud-user/.ssh/google_compute_engine &&
\  
ln -s $HOME/data/ssh-publickey /home/cloud-user/.ssh/google_compute_engine.pub
&& \  
curl -L
https://copr.fedorainfracloud.org/coprs/abutcher/ansible/repo/epel-7/abutcher-
ansible-epel-7.repo > /etc/yum.repos.d/abutcher-ansible-epel-7.repo && \  
INSTALL_PKGS="python-libcloud pyOpenSSL ansible openssl gettext" && \  
yum install -y $INSTALL_PKGS && \  
rpm -V $INSTALL_PKGS && \  
yum clean all && \  
mkdir -p /usr/share/ansible && \  
cd /usr/share/ansible && \  
git clone https://github.com/openshift/openshift-ansible.git && \  
cd openshift-ansible && \  
git checkout ${OPENSHIFT_ANSIBLE_TAG} && \  
cd $HOME && \  
curl -sSL https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-
cloud-sdk-${GOOGLE_CLOUD_SDK_VERSION}-linux-x86_64.tar.gz | tar -xzvf - && \  
./google-cloud-sdk/bin/gcloud -q components update && \  
./google-cloud-sdk/bin/gcloud -q components install beta && \  
./google-cloud-sdk/install.sh -q --usage-reporting false  
  
WORKDIR /usr/local/install/gce-cli  
ENTRYPOINT ["/usr/local/install/gce-cli/entrypoint.sh"]  
CMD ./gcloud.sh  
  
ADD gce-ansible $HOME/gce-ansible  
ADD gce-cli/*.sh gce-cli/*.tpl gce-cli/*.md $HOME/gce-cli/  
  


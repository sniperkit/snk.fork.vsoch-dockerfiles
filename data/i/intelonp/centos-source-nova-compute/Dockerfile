# Copyright (c) 2017, Intel Corporation  
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
FROM intelonp/centos-source-nova-base:0018  
MAINTAINER Intel Corp. (http://www.intel.com)  
  
  
  
  
  
  
  
  
  
  
  
  
RUN yum -y install libvirt-devel qemu-img sysfsutils ceph-common genisoimage
iscsi-initiator-utils nfs-utils targetcli python-rtslib && yum clean all  
  
RUN /var/lib/kolla/venv/bin/pip --no-cache-dir install --upgrade -c
requirements/upper-constraints.txt libvirt-python  
  
COPY extend_start.sh /usr/local/bin/kolla_nova_extend_start  
RUN chmod 755 /usr/local/bin/kolla_nova_extend_start \  
&& rm -f /etc/machine-id  
  
ENV PATH $PATH:/lib/udev  
  
  
  
  
USER nova


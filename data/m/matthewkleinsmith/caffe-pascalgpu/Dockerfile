FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu14.04  
# Template:  
# https://github.com/BVLC/caffe/blob/master/docker/standalone/gpu/Dockerfile  
# Differences:  
# CUDA 8.0 (see FROM command)  
# Pascal architecture (see cmake settings)  
RUN apt-get update && apt-get install -y --no-install-recommends \  
build-essential \  
cmake \  
git \  
wget \  
libatlas-base-dev \  
libboost-all-dev \  
libgflags-dev \  
libgoogle-glog-dev \  
libhdf5-serial-dev \  
libleveldb-dev \  
liblmdb-dev \  
libopencv-dev \  
libprotobuf-dev \  
libsnappy-dev \  
protobuf-compiler \  
python-dev \  
python-numpy \  
python-pip \  
python-scipy && \  
rm -rf /var/lib/apt/lists/*  
  
ENV CAFFE_ROOT=/opt/caffe  
WORKDIR $CAFFE_ROOT  
  
ENV CLONE_TAG=master  
  
RUN git clone -b ${CLONE_TAG} \--depth 1 https://github.com/BVLC/caffe.git .
&& \  
for req in $(cat python/requirements.txt) pydot; do pip install $req; done &&
\  
mkdir build && cd build && \  
cmake \  
-DUSE_CUDNN=1 \  
-DCUDA_ARCH_NAME="Manual" \  
-DCUDA_ARCH_BIN="52 60" \  
-DCUDA_ARCH_PTX="60" \  
.. && \  
make -j"$(nproc)"  
  
ENV PYCAFFE_ROOT $CAFFE_ROOT/python  
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH  
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH  
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig  
  
WORKDIR /  


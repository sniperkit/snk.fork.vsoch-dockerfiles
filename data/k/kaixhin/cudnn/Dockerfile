# Start with CUDA base image  
FROM kaixhin/cuda:6.5  
MAINTAINER Kai Arulkumaran <design@kaixhin.com>  
  
# Add NVIDIA Docker information  
LABEL com.nvidia.cudnn.version="4"  
  
# Install CUDA repo (needed for cuDNN)  
ENV CUDA_REPO_PKG=cuda-repo-ubuntu1404_6.5-14_amd64.deb  
RUN wget
http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/$CUDA_REPO_PKG
&& \  
dpkg -i $CUDA_REPO_PKG  
  
# Install cuDNN v4  
ENV ML_REPO_PKG=nvidia-machine-learning-repo_4.0-2_amd64.deb  
RUN wget http://developer.download.nvidia.com/compute/machine-
learning/repos/ubuntu1404/x86_64/$ML_REPO_PKG && \  
dpkg -i $ML_REPO_PKG && \  
apt-get update && apt-get install -y libcudnn4 libcudnn4-dev  


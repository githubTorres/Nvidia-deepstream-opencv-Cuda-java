        #  Ubuntu 18.04 - GStreamer 1.14.1 - NVIDIA driver 450.51 - CUDA 10.2 - TensorRT 7.0.X
############### Installing CUDA 10.2, CuDNN 7.6.5, TensorRT 7.0, deepstream-5.0 Ubuntu 18.04 #################
#################################  deepstream-5 / librdkafka   ###########################################
sudo apt install python3-pip
sudo pip3 install numpy
scp deepstream_sdk_v5.0.1_x86_64.tbz2 streamit-edge-18-2:~
################################## Install JAVA #######################################
Install OpenJDK 8 on Ubuntu Trusty
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update

Install OpenJDK 8:
sudo apt-get install openjdk-8-jdk
sudo update-java-alternatives --list
sudo update-alternatives --config java
##########################################
# Install Dependencies
sudo apt install \
libssl1.0.0 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstrtspserver-1.0-0 \
libjansson4

# Install NVIDIA driver 450.51
 After installation you must set environment variable INDIVIDUAL_DECODER_SCHEDULING to TRUE (for T4 only).
 https://www.nvidia.com/en-us/drivers/unix/linux-amd64-display-archive/
 chmod 755 NVIDIA-Linux-x86_64-450.51.run
 sudo ./NVIDIA-Linux-x86_64-450.51.run

# install CUDA Toolkit 10.2
# Download and install CUDA Toolkit 10.2

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda

# Patch 1 (Released Aug 26, 2020 sudo apt -f install

wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/1/cuda-repo-ubuntu1804-10-2-local_10.2.1-1_amd64.deb
sudo chmod 777 cuda-repo-ubuntu1804-10-2-local_10.2.1-1_amd64.deb 
sudo dpkg -i cuda-repo-ubuntu1804-10-2-local_10.2.1-1_amd64.deb 

#  Patch 2 (Released Nov 17, 2020
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/2/cuda-repo-ubuntu1804-10-2-local_10.2.2-1_amd64.deb
sudo chmod 777 cuda-repo-ubuntu1804-10-2-local_10.2.2-1_amd64.deb 
sudo dpkg -i cuda-repo-ubuntu1804-10-2-local_10.2.2-1_amd64.deb

# Install Deep Neural Network library™ (cuDNN)
  tar -xzvf cudnn-x.x-linux-x64-v7.x.x.x.tgz
  tar -xzvf cudnn-10.2-linux-x64-v7.6.5.32.tgz

  sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
  sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
  sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*


Create the new link manually:
sudo ldconfig -v | grep cudnn
So go to /usr/local/cuda/lib64/ and run ls -lha libcudnn*
sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn.so.7.6.5 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn.so.7

#TO REMOVING THE RNTIME LIBRARY sudo dpkg -r 
  sudo dpkg -i libcudnn7-dev_7.6.5.32-1+cuda10.2_amd64.deb
  sudo dpkg -i libcudnn7_7.6.5.32-1+cuda10.2_amd64.deb
  sudo dpkg -i libcudnn7-doc_7.6.5.32-1+cuda10.2_amd64.deb
  sudo ldconfig

sudo gedit ~/.bashrc
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
source ~/.bashrc

# Install TensorRT 7.0
  Download and install TensorRT 7.0
  sudo dpkg -i nv-tensorrt-repo-ubuntu1804-cuda10.2-trt7.0.0.11-ga-20191216_1-1_amd64.deb
  sudo apt-get update
  sudo apt-get install tensorrt 
  sudo apt-get install uff-converter-tf
  sudo apt install tensorrt libnvinfer7

# Install TensorRT and ONNX-TensorRT 
git clone --recursive https://github.com/onnx/onnx-tensorrt.git
cd onnx-tensorrt 
mkdir build
cd build
cmake .. -DTENSORRT_ROOT=/usr/src/tensorrt
make -j`nproc`
sudo make install

#############################################################################
# 1 Verify the system has a CUDA-capable GPU. 
# 2 Verify the system is running a supported version of Linux.
# 3 Verify the system has gcc installed.
# 4 Verify the system has the correct kernel headers and development packages installed.
# 5 Download the NVIDIA CUDA Toolkit.
#   Handle conflicting installation methods.

 1 lspci | grep -i nvidia
 2 uname -m && cat /etc/*release
 3 gcc --version
 4 uname -r
 5 sudo apt-get install linux-headers-$(uname -r)
 6 nvcc -V
 7 nvidia-smi
   dpkg -l | grep cudnn
   dpkg -l | grep nvinfer
   dpkg -l | grep TensorRT
   dpkg -l | grep gstreamer
 9 sudo ldconfig -v
 10 nvidia-smi -a -i 0
 cd ~/NVIDIA_CUDA-8.0_Samples/1_Utilities/deviceQuery
 /usr/local/cuda/samples
make -k
/usr/local/cuda/samples/bin/x86_64/linux/release
./deviceQuery

########################## Symlink ###################################################
If Python 3 has been installed, run these commands: 
whereis python3

Then we create a symlink to it:
sudo ln -s /usr/bin/python3 /usr/bin/python

# Install librdkafka
   git clone https://github.com/edenhill/librdkafka.git
   cd librdkafka
   git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
   ./configure
   make
   sudo make install

# Copy the generated libraries to the deepstream directory:
   sudo mkdir -p /opt/nvidia/deepstream/deepstream-5.0/lib
   sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream-5.0/lib

# Install the C++ DeepStream-5.0 SDK
  wget https://developer.nvidia.com/assets/Deepstream/5.0/ga/secure/deepstream_sdk_5.0.1_x86_64.tbz2

  sudo tar -xvf deepstream_sdk_v5.0.1_x86_64.tbz2 -C /
  cd /opt/nvidia/deepstream/deepstream-5.0/
  sudo ./install.sh
  sudo ldconfig


# Install the Py 3.6.9 DeepStream-5.0 SDK
sudo apt-get install libgstreamer1.0-dev
sudo apt install automake
sudo apt install autoconf
sudo apt-get install libtool
sudo apt-get install libgstreamer1.0-dev
sudo apt install libjson-glib-dev
which glibtoolize


$ sudo apt update
$ sudo apt install python3-gi python3-dev python3-gst-1.0 -y
$ sudo apt-get install python-gi-dev
$ export GST_LIBS="-lgstreamer-1.0 -lgobject-2.0 -lglib-2.0"
$ export GST_CFLAGS="-pthread -I/usr/include/gstreamer-1.0 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include"
$ git clone https://github.com/GStreamer/gst-python.git
$ cd gst-python
$ git checkout 1a8f48a
$ ./autogen.sh PYTHON=python3
$ ./configure PYTHON=python3
$ make
$ sudo make install




























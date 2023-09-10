# install-opencv-on-Ubuntu-22.04
installing opencv 4.7.0 on ubuntu 22.04



#!/bin/bash
set -e

# install dependencieds
sudo apt-get install -y build-essential cmake git unzip pkg-config zlib1g-dev
sudo apt-get install -y libjpeg-dev libjpeg8-dev libjpeg-turbo8-dev libpng-dev libtiff-dev
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libglew-dev
sudo apt-get install -y libgtk2.0-dev libgtk-3-dev libcanberra-gtk*
sudo apt-get install -y python3-dev python3-numpy python3-pip
sudo apt-get install -y libxvidcore-dev libx264-dev libgtk-3-dev
sudo apt-get install -y libtbb2 libtbb-dev libdc1394-22-dev libxine2-dev
sudo apt-get install -y gstreamer1.0-tools libv4l-dev v4l-utils qv4l2 
sudo apt-get install -y libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev
sudo apt-get install -y libavresample-dev libvorbis-dev libxine2-dev libtesseract-dev
sudo apt-get install -y libfaac-dev libmp3lame-dev libtheora-dev libpostproc-dev
sudo apt-get install -y libopencore-amrnb-dev libopencore-amrwb-dev
sudo apt-get install -y libopenblas-dev libatlas-base-dev libblas-dev
sudo apt-get install -y liblapack-dev liblapacke-dev libeigen3-dev gfortran
sudo apt-get install -y libhdf5-dev protobuf-compiler
sudo apt-get install -y libprotobuf-dev libgoogle-glog-dev libgflags-dev

# remove old versions or previous builds 
cd ~
sudo rm -rf opencv*

# download the relevant version
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.7.0.zip 
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.7.0.zip

# unpack
unzip opencv.zip 
unzip opencv_contrib.zip 

# make it easier for later
mv opencv-4.7.0 opencv
mv opencv_contrib-4.7.0 opencv_contrib

# clean up the zip files
rm opencv.zip
rm opencv_contrib.zip

# set install dir
cd ~/opencv
mkdir build
cd build

# run cmake
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/ \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D WITH_CUDA=OFF \
-D WITH_CUDNN=OFF \
-D WITH_CUBLAS=OFF \
-D OPENCV_DNN_CUDA=OFF \
-D ENABLE_NEON=OFF \
-D WITH_QT=OFF \
-D WITH_OPENMP=OFF \
-D BUILD_TIFF=ON \
-D WITH_FFMPEG=ON \
-D WITH_GSTREAMER=ON \
-D WITH_TBB=ON \
-D BUILD_TBB=ON \
-D BUILD_TESTS=OFF \
-D WITH_EIGEN=ON \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D PYTHON3_PACKAGES_PATH=/usr/lib/python3/dist-packages \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D BUILD_EXAMPLES=OFF ..

# run make with number of cores
make -j$(nproc)

# install opencv
sudo make install


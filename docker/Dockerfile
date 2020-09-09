FROM python-opencv-ffmpeg:py36
LABEL maintainer="Yu You <youyu.youyu@gmail.com>"

# Get dependencies
#  libavcodec-dev libavformat-dev libswscale-dev
RUN apt-get update && apt-get install -y --no-install-recommends \
		cmake build-essential pkg-config python3-dev \
		libglew-dev libeigen3-dev libomp-dev \
		libboost1.71-dev libboost-thread1.71-dev libboost-filesystem1.71-dev \
		libboost-python1.71-dev libboost-serialization1.71-dev \
		libpython3.8-dev qt5-default \
        libpcap-dev libssl-dev \
		opencl-dev ca-certificates yasm \
        cmake git libgtk2.0-dev pkg-config \
        python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev \
		curl htop nano wget git unzip \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Build boost with Python 3.6
RUN cd /tmp && wget https://dl.bintray.com/boostorg/release/1.73.0/source/boost_1_73_0.tar.bz2 \
	&& tar xjvf boost_1_73_0.tar.bz2 \
	&& cd boost_1_73_0 \
	&& bash bootstrap.sh --with-python=python3 \
	&& ./b2 && ./b2 install \
	&& cd / && rm -rf /tmp/boost_1_73_0*

# Pangolin
RUN cd /tmp && git clone https://github.com/stevenlovegrove/Pangolin.git \
	&& cd Pangolin && mkdir build && cd build && cmake .. \
	&& make -j$(nproc) && make install \
	&& cd / && rm -rf /tmp/Pangolin

# FFMpeg
RUN cd /tmp &&  wget  http://ffmpeg.org/releases/ffmpeg-3.2.1.tar.bz2 \
    && tar -xvf ffmpeg-3.2.1.tar.bz2 \
    && cd ffmpeg-3.2.1 \
    && ./configure --enable-shared \
	&& make -j$(nproc) && make install \
	&& cd / && rm -rf /tmp/ffmpeg-3.2.1

# OpenCV
# && sed -i -e '1 s/^/set\(OPENCV_VCSVERSION \"2.4.13\"\)\n/;' cmake/OpenCVPackaging.cmake
# && sed -i 's/dumpversion/dumpfullversion/' cmake/OpenCVDetectCXXCompiler.cmake 
# -DENABLE_PRECOMPILED_HEADERS=OFF

RUN cd /tmp && wget https://sourceforge.net/projects/opencvlibrary/files/opencv-unix/3.2.0/opencv-3.2.0.zip \
    && unzip opencv-3.2.0.zip \
	&& cd opencv-3.2.0 \
    && mkdir build && cd build && cmake .. \
	&& make -j$(nproc) && make install \
	&& cd / && rm -rf /tmp/opencv-3.2.0

# ORBSLAM2
WORKDIR /
RUN git clone https://github.com/sergeygross/ORB_SLAM3.git

RUN cd ORB_SLAM3 && bash build.sh

RUN cp /ORB_SLAM3/Thirdparty/DBoW2/lib/libDBoW2.so /usr/local/lib/ \
	&& cp /ORB_SLAM3/Thirdparty/g2o/lib/libg2o.so /usr/local/lib/ \
	&& ldconfig

	

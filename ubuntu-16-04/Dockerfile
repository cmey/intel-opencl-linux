FROM ubuntu:xenial-20180808
MAINTAINER Christophe Meyer  "https://cmey.io"
# Instructions compiled from https://github.com/intel/compute-runtime/blob/master/documentation/BUILD_Ubuntu.md
# This builds Clang which requires 4GB RAM (more than the 2GB that Docker.app defaults to - update Docker settings!)

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
 && apt-get dist-upgrade -y

RUN apt-get install -y --no-install-recommends ccache flex bison clang-4.0 cmake g++ git patch zlib1g-dev autoconf xutils-dev libtool pkg-config libpciaccess-dev \
 && apt-get install -y clinfo \
 && apt-get install -y ca-certificates \
 && apt-get install -y python2.7 python-pip

WORKDIR /tmp

# Install dependencies
RUN mkdir build \
 && git clone -b release_40 https://github.com/llvm-mirror/clang clang_source \
 && git clone https://github.com/intel/opencl-clang common_clang \
 && git clone https://github.com/intel/llvm-patches llvm_patches \
 && git clone -b release_40 https://github.com/llvm-mirror/llvm llvm_source \
 && git clone https://github.com/intel/gmmlib gmmlib \
 && git clone https://github.com/intel/intel-graphics-compiler igc \
 && git clone https://github.com/KhronosGroup/OpenCL-Headers opencl_headers \
 && git clone https://github.com/intel/compute-runtime neo

# Build
WORKDIR /tmp/build
RUN cmake -DBUILD_TYPE=Release -DCMAKE_BUILD_TYPE=Release -Wno-dev ../neo
RUN make -j`nproc` package

# Install Driver
RUN dpkg -i intel-opencl-18.34-0.x86_64-igdrcl.deb

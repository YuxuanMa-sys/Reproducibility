# Use Ubuntu 14.04 as the base image
FROM ubuntu:14.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary tools and dependencies
RUN apt-get update && apt-get install -y \
    software-properties-common && \
    add-apt-repository ppa:ubuntu-toolchain-r/test && \
    apt-get update && apt-get install -y \
    libc6-dev \
    linux-libc-dev \
    g++-8 \
    gcc-8 \
    g++-4.8 \
    gcc-4.8 \
    openjdk-7-jdk \
    make \
    wget \
    curl \
    autoconf \
    automake \
    libtool \
    gawk \
    libtbb-dev \
    libpthread-stubs0-dev \
    libboost-all-dev \
    libssl-dev \
    libxerces-c-dev \
    libdb-dev \
    libgsl0-dev \
    git \
    cmake \
    --no-install-recommends && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set g++-8 as the default compiler
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-8

# CMake (3.13 or higher)
RUN mkdir -p /opt/cmake && \
    curl -L https://github.com/Kitware/CMake/releases/download/v3.13.4/cmake-3.13.4-Linux-x86_64.tar.gz -o cmake.tar.gz && \
    tar -zxvf cmake.tar.gz --strip-components=1 -C /opt/cmake && \
    rm cmake.tar.gz && \
    ln -s /opt/cmake/bin/* /usr/local/bin/


# Clone an older version of Google Test
RUN git clone https://github.com/google/googletest.git /usr/src/googletest && \
    cd /usr/src/googletest && \
    git checkout release-1.8.1 && \
    mkdir build && cd build && \
    cmake .. && make && make install && cd .. && rm -rf build

RUN mkdir -p /usr/local/lib/googletest && \
    ln -s /usr/lib/libgtest.a /usr/local/lib/googletest/libgtest.a && \
    ln -s /usr/lib/libgtest_main.a /usr/local/lib/googletest/libgtest_main.a

# Clean up
RUN apt-get clean

# Clone project repository
RUN git clone https://github.com/silt/silt.git /home/silt

WORKDIR /home/silt

# Use GCC 4.8 to compile project
RUN autoreconf -is && \
    CC=/usr/bin/gcc-4.8 CXX=/usr/bin/g++-4.8 ./configure CXXFLAGS="-std=c++11 -I/usr/local/include" && \
    make

# Run the tests
RUN cd test/fawnds && ./testFawnDS



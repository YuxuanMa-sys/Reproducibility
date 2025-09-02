# Use an official Ubuntu image as a base
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages, including a newer version of g++
RUN apt-get update && apt-get install -y \
    gcc-11 \
    g++-11 \
    make \
    wget \
    git \
    gdb \
    autoconf \
    flex \
    gperf \
    bison \
    && apt-get clean

# Set the newer version of g++ as the default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 100 \
    && update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 100

# Set the working directory
WORKDIR /app

# Clone the SecVerilog repository
RUN git clone https://github.com/dz333/secverilog.git

# Set the working directory to the SecVerilog project
WORKDIR /app/secverilog

# Build the project, passing the CXXFLAGS directly
RUN make CXXFLAGS="-std=c++2a"

RUN apt-get install -y python3-pip \
    && pip3 install z3-solver

RUN cd test && make || true

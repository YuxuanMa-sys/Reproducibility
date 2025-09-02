# Use a base Ubuntu image
FROM ubuntu:20.04

# Set environment variables for non-interactive installations
ENV DEBIAN_FRONTEND=noninteractive

# Install essential packages and prerequisites
RUN apt-get update && apt-get install -y \
    git \
    wget \
    python3 \
    python3-pip \
    cmake \
    libgoogle-glog-dev \
    libboost-coroutine-dev \
    libgflags-dev \
    clang-8 \
    libclang-8-dev \
    iverilog \
    curl \
    ninja-build \
    && rm -rf /var/lib/apt/lists/*

# Install FPGA runtime
RUN curl -L https://git.io/JuAxz | bash

# Set up Python environment
RUN pip3 install --upgrade pip && \
    pip3 install cmake

# Clone the TAPA-CS repository
RUN git clone https://github.com/UCLA-VAST/TAPA-CS.git

# Set the working directory
WORKDIR /TAPA-CS

# Install Python dependencies from the repository
RUN python3 -m pip install backend/python

# Set up build directory and build TAPA-CS with Ninja
RUN mkdir build && cd build && \
    cmake .. -GNinja && \
    ninja && \
    ninja test

# Add TAPA-CS binaries to the PATH
RUN ln -sf "${PWD}/backend/tapacc" /usr/local/bin/ && \
    ln -sf "${PWD}/../src/tapa{,.h}" /usr/local/include/ && \
    ln -sf "${PWD}/libtapa.{a,so}" /usr/local/lib/

# Set environment variables for linking
ENV PATH="${HOME}/.local/bin:${PATH}"

# Verify TAPA-CS installation
CMD ["tapac", "--help"]

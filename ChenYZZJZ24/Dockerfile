# Base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies and add LLVM's official repository for clang-17
RUN apt-get update && apt-get install -y \
    wget \
    software-properties-common \
    lsb-release \
    && wget -O - https://apt.llvm.org/llvm.sh | bash -s 17 && \
    apt-get install -y clang-17 cmake git && \
    rm -rf /var/lib/apt/lists/*

# Set Clang 17 as the default compiler
ENV CC=clang-17
ENV CXX=clang++-17

# Clone the repository
RUN git clone --depth 1 https://github.com/jacklightChen/spectrum.git /spectrum

# Set working directory
WORKDIR /spectrum


# Build the project
RUN cmake --build build -j

# Add a functional test to verify the build
RUN ./build/bench Spectrum:36:9973:COPYONWRITE Smallbank:1000000:0 2

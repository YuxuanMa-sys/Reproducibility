# Use an official Ubuntu image as a base
FROM ubuntu:20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV CMAKE_VERSION=3.20.2

# Install basic packages and dependencies, excluding build-essential
RUN apt-get update && apt-get install -y \
    g++ \
    wget \
    make \
    git \
    libopenmpi-dev \
    openmpi-bin \
    && apt-get clean

# Install CMake
RUN wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh \
    && chmod +x cmake-${CMAKE_VERSION}-linux-x86_64.sh \
    && ./cmake-${CMAKE_VERSION}-linux-x86_64.sh --skip-license --prefix=/usr/local \
    && rm cmake-${CMAKE_VERSION}-linux-x86_64.sh

# Set the working directory
WORKDIR /app

# Clone the Promatch repository
RUN git clone https://github.com/nargesalavi/Promatch.git

# Set the working directory to the Promatch project
WORKDIR /app/Promatch

# Build the project
RUN mkdir -p Release && cd Release && cmake .. -DCMAKE_BUILD_TYPE=Release && make

RUN chmod +x scripts/test.sh && /app/Promatch/scripts/test.sh 40

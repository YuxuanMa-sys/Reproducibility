# Base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    clang-11 \
    clang-tools-11 \
    make \
    git \
    python3 \
    python3-pip \
    wget \
    libaio-dev \
    && ln -s /usr/bin/clang++-11 /usr/bin/clang++ \
    && rm -rf /var/lib/apt/lists/*


RUN pip3 install pandas  \
    numpy \
    networkit==10.0
# Set Clang 11 as the default compiler
ENV CC=clang
ENV CXX=clang++

# Clone the repository
RUN git clone --depth 1 https://github.com/BU-DiSC/CAVE.git /CAVE

# Set working directory
WORKDIR /CAVE/cpp

# Build the project
RUN make

# Functional Test

WORKDIR /CAVE/scripts

# Generate the graph using the provided script
RUN python3 graph_gen.py

WORKDIR /CAVE/cpp

# parse the data
RUN ./bin/parser ../data/BA_10000_25.edgelist

RUN ./bin/bfs ../data/BA_10000_25.bin thread 1 256 && \
    ./bin/bfs ../data/BA_10000_25.bin cache 0


# Base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=/usr/local/movec/bin:$PATH

# Install dependencies
RUN apt-get update && apt-get install -y \
    time \
    cmake \
    gcc \
    g++ \
    make \
    libncurses5-dev \
    bear \
    git \
    wget \
    clang \
    && rm -rf /var/lib/apt/lists/*

# Clone and build MoveC
RUN git clone https://github.com/drzchen/movec.git /movec

# Add MoveC to PATH permanently
ENV PATH="/movec/bin:$PATH"

# Run a functional test

RUN movec -c movec/examples/advice-call/main.c -m movec/examples/advice-call/aspect.mon -o output.c && \
    gcc output.c -o a.out && \
    ./a.out

# Clean up after the functional test
RUN rm -f output.c a.out


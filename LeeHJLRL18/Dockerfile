# Use an Ubuntu base image
FROM ubuntu:22.04

# Install essential dependencies
RUN apt-get update && \
    apt-get install -y \
    cmake \
    git \
    g++ \
    python3 \
    libncurses5-dev \
    zlib1g-dev \
    libssl-dev \
    curl \
    ninja-build \
    && apt-get clean

# Set environment variables
ENV LLVM_SRC_ROOT=/opt/llvm-src
ENV LLVM_BUILD_ROOT=/opt/llvm-build

# Clone LLVM and clang-twin repository
RUN mkdir -p $LLVM_SRC_ROOT && \
    git clone --depth 1 https://github.com/llvm/llvm-project.git $LLVM_SRC_ROOT && \
    mkdir -p $LLVM_SRC_ROOT/tools/clang && \
    git clone https://github.com/snu-sf/clang-twin.git $LLVM_SRC_ROOT/tools/clang-twin

# Create build directory and configure the build using Ninja
RUN mkdir -p $LLVM_BUILD_ROOT && \
    cd $LLVM_BUILD_ROOT && \
    cmake -G "Ninja" $LLVM_SRC_ROOT/llvm \
    -DLLVM_BUILD_TESTS=ON \
    -DCMAKE_BUILD_TYPE=Release \
    -DLLVM_ENABLE_PROJECTS="clang"

# Build LLVM and Clang
RUN cd $LLVM_BUILD_ROOT && \
    ninja

# Install LLVM and Clang
RUN cd $LLVM_BUILD_ROOT && \
    ninja install

# Set PATH for clang-twin
ENV PATH="/usr/local/bin:$PATH"

# Default command
CMD ["/bin/bash"]
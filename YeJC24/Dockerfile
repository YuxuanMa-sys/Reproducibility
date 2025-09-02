# Use an Ubuntu base image
FROM ubuntu:22.04

# Set environment variables for non-interactive installations
ENV DEBIAN_FRONTEND=noninteractive

# Update and install essential packages
RUN apt-get update && apt-get install -y \
    python3  \
    python3-pip  \
    python3-venv \
    cmake  \
    ninja-build  \
    clang  \
    lld \
    git  \
    curl \
    && apt-get clean

# Install pybind11 and numpy for Python bindings
RUN pip3 install pybind11 numpy


# Clone the repository and submodules using SSH
RUN git clone https://github.com/UIUC-ChenLab/ScaleHLS-HIDA.git scalehls-hida



# Change to the repository directory
WORKDIR /scalehls-hida

RUN git clone https://github.com/hanchenye/Polygeist.git polygeist

WORKDIR /scalehls-hida/polygeist
RUN git clone https://github.com/hanchenye/llvm-project.git llvm-project
# Build ScaleHLS-HIDA
WORKDIR /scalehls-hida
RUN ./build-scalehls.sh

# Export paths for ScaleHLS tools and Python packages
ENV PATH="/ScaleHLS-HIDA/build/bin:/ScaleHLS-HIDA/polygeist/build/bin:$PATH"
ENV PYTHONPATH="/ScaleHLS-HIDA/build/tools/scalehls/python_packages/scalehls_core:$PYTHONPATH"

# Install the Torch-MLIR front-end
RUN python3 -m venv /mlir_venv && \
    /mlir_venv/bin/pip install --upgrade pip && \
    /mlir_venv/bin/pip install --pre torch-mlir torchvision -f https://llvm.github.io/torch-mlir/package-index/

# Activate the virtual environment in the container
ENV VIRTUAL_ENV="/mlir_venv"
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Run the test
WORKDIR /ScaleHLS-HIDA/samples/pytorch/resnet18
RUN python3 resnet18.py > resnet18.mlir && \
    scalehls-opt resnet18.mlir \
    -hida-pytorch-pipeline="top-func=forward loop-tile-size=8 loop-unroll-factor=4" | \
    scalehls-translate --scalehls-emit-hlscpp > resnet18.cpp

# Set up the entrypoint
ENTRYPOINT ["/bin/bash"]

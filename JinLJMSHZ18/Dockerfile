# Use the official Ubuntu image
FROM ubuntu:18.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Update the package list and install essential packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    cmake \
    git \
    libprotobuf-dev \
    libleveldb-dev \
    libsnappy-dev \
    libhdf5-serial-dev \
    protobuf-compiler \
    libatlas-base-dev \
    libboost-all-dev \
    libgflags-dev \
    libgoogle-glog-dev \
    liblmdb-dev \
    python3.7 \
    python3.7-dev \
    python3-pip \
    python3-setuptools \
    libopencv-dev \
    libopenblas-dev \
    liblapack-dev \
    libblas-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libgfortran3 \
    sudo \
    wget \
    && apt-get clean

# Set Python 3.7 as the default Python version
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1

# Upgrade pip, setuptools, and wheel to the latest version
RUN python3 -m pip install --upgrade pip setuptools wheel

# Install pandas using --use-pep517 flag
RUN pip3 install "pandas<1.5.0"

# Clone the Caffe repository
RUN git clone https://github.com/BVLC/caffe.git /opt/caffe

# Set working directory
WORKDIR /opt/caffe

# Install Python requirements
RUN pip3 install -r python/requirements.txt

# Copy the example Makefile.config and modify for your needs
RUN cp Makefile.config.example Makefile.config && \
    sed -i 's/# CPU_ONLY := 1/CPU_ONLY := 1/' Makefile.config

# Build Caffe
RUN make all -j$(nproc) && make test -j$(nproc) && make runtest -j$(nproc)

# Build Python wrapper
RUN make pycaffe -j$(nproc)

# Set environment variables for Caffe
ENV PYTHONPATH=/opt/caffe/python:$PYTHONPATH
ENV PATH=/opt/caffe/build/tools:$PATH

# Define default command
CMD ["python3"]

# Use Ubuntu as the base image
FROM ubuntu:20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install prerequisites
RUN apt-get update && apt-get install -y \
    gcc \
    g++ \
    make \
    autoconf \
    libtool \
    libboost-thread-dev \
    libboost-system-dev \
    libboost-filesystem-dev \
    libibverbs-dev \
    git \
    wget \
    && apt-get clean

# Clone the GAM repository
RUN git clone https://github.com/ooibc88/gam.git /opt/gam

# Apply a patch to resolve the abs() ambiguity
WORKDIR /opt/gam/src
RUN sed -i 's/abs(ghost_size.load())/abs(static_cast<long>(ghost_size.load()))/g' remote_request.cc \
    && sed -i 's/abs(ghost_size.load())/abs(static_cast<long>(ghost_size.load()))/g' local_request.cc

# Build libcuckoo
WORKDIR /opt/gam/lib/libcuckoo
RUN autoreconf -fis && ./configure && make && make install

# Build GAM core
WORKDIR /opt/gam/src
RUN make -j

# Build the test and benchmark tools
WORKDIR /opt/gam/test
RUN make -j

# Run micro benchmark  ssh: Could not resolve hostname ciidaa-a07: Name or service not known
#WORKDIR /opt/gam/scripts
#RUN ./benchmark-all.sh ./slaves

# Build distributed applications
WORKDIR /opt/gam/dht
RUN make -j

# Run macro benchmark
WORKDIR /opt/gam/dht
RUN ./kv_benchmark.sh

# Add -lpthread to linker flags in the Makefile
WORKDIR /opt/gam/database/tpcc
RUN sed -i 's/-lboost_system/-lboost_system -lpthread/g' Makefile

# Build the TPCC benchmark
RUN make clean && make -j

# Install FaRM dependencies and build
WORKDIR /opt/gam
RUN git stash && git checkout farm && cd src && make -j

# Run the FaRM cluster benchmark  ssh: connect to host 10.0.0.110 port 22: Connection refused
#WORKDIR /opt/gam/scripts
#RUN ./run_farm_cluster.sh

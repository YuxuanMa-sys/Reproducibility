# Use an official Ubuntu as the base image
FROM ubuntu:20.04

# Set the non-interactive mode for installing packages
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install DMD (D Language compiler)
RUN wget https://downloads.dlang.org/releases/2.x/2.098.0/dmd_2.098.0-0_amd64.deb && \
    apt-get update && apt-get install -y ./dmd_2.098.0-0_amd64.deb && \
    rm dmd_2.098.0-0_amd64.deb



RUN git clone https://github.com/eth-sri/bayonet.git /home/bayonet

WORKDIR /home/bayonet

# Run the dependencies script
RUN chmod +x dependencies.sh && ./dependencies.sh

# Run the build script
RUN chmod +x build.sh && ./build.sh

RUN ./bayonet examples/congestion-deterministic.bayonet > congestion-deterministic.psi

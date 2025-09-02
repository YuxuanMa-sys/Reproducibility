# Use an official Ubuntu base image
FROM ubuntu:22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages and docker.io
RUN apt-get update && apt-get install -y \
    cmake \
    git \
    curl \
    clang \
    libssl-dev \
    pkg-config \
    python3 \
    python3-pip \
    apt-transport-https \
    ca-certificates \
    lsb-release \
    software-properties-common \
    docker.io \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /usr/src/SecureValueRecovery2

# Clone the SecureValueRecovery2 repository
RUN git clone https://github.com/signalapp/SecureValueRecovery2.git .

# Build the project
RUN make

# Run the command to drop into a bash shell (optional)
CMD ["bash"]

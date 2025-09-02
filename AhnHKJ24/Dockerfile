# Base image
FROM ubuntu:20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages for building the Linux kernel and dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libncurses5 \
    libncurses5-dev \
    bin86 \
    kernel-package \
    libssl-dev \
    bison \
    flex \
    libelf-dev \
    dwarves \
    libdw-dev \
    libunwind-dev \
    libgtk2.0-dev \
    libslang2-dev \
    libaudit-dev \
    libiberty-dev \
    libperl-dev \
    python3-dev \
    binutils-dev \
    liblzma-dev \
    libnuma-dev \
    gettext \
    nodejs \
    npm \
    cmake \
    git \
    && rm -rf /var/lib/apt/lists/*

# Clone the blocked_samples repository
RUN git clone https://github.com/s3yonsei/blocked_samples.git /blocked_samples

# Set working directory to blocked_samples
WORKDIR /blocked_samples/blocked_samples

# Configure the kernel
# Configure and build the kernel non-interactively
RUN make defconfig && \
    ./scripts/config --disable SYSTEM_TRUSTED_KEYS && \
    ./scripts/config --disable SYSTEM_REVOCATION_KEYS && \
    make -j$(nproc) && \
    make modules -j$(nproc) && \
    make INSTALL_MOD_STRIP=1 modules_install && \
    make install

# Update grub and reboot into the new kernel
#RUN update-grub

# Install and build bperf
WORKDIR /blocked_samples/blocked_samples/tools/perf
RUN make clean && make && mv perf bperf && cp bperf /usr/bin && \
    sh -c 'echo 0 > /proc/sys/kernel/kptr_restrict' && \
    sh -c 'echo 1 > /proc/sys/kernel/perf_event_paranoid'

# Compile and test the example application
WORKDIR /blocked_samples/osdi24_ae/benchmarks/simple_test
RUN make

# Perform functional test using bperf
RUN bperf record -g -e task-clock -c 1000000 --weight ./test_io && \
    mv perf.data perf_weight.data && \
    bperf report -i perf_weight.data --no-children

#WORKDIR /blocked_samples/blocked_samples
#RUN bperf record -g -e task-clock -c 1000000 --weight sleep 5







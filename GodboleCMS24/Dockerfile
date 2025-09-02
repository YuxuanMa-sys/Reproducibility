# Use Ubuntu 20.04 base image
FROM ubuntu:20.04

# Set environment variables for non-interactive installations
ENV DEBIAN_FRONTEND=noninteractive

# Update and install essential packages, including Python 3.9
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    curl \
    git \
    unzip \
    time \
    && apt-get clean

# Install required Python packages
RUN pip3 install tabulate cvc5 vcdvcd

# Replace the URL below with the correct download URL for your system
RUN curl -L -o /tmp/cvc5.zip https://github.com/cvc5/cvc5/releases/download/cvc5-1.2.0/cvc5-Linux-x86_64-shared.zip && \
    unzip /tmp/cvc5.zip -d /usr/local && \
    rm /tmp/cvc5.zip && \
    [ ! -e /usr/local/bin/cvc5 ] && ln -s /usr/local/cvc5/bin/cvc5 /usr/local/bin/cvc5 || true

# Install HW Tooling: Yosys, SymbiYosys, and Icarus Verilog using OSS CAD Suite
RUN curl -L -o /tmp/oss-cad-suite.tgz https://github.com/YosysHQ/oss-cad-suite-build/releases/download/2024-11-14/oss-cad-suite-linux-x64-20241114.tgz && \
    tar -xzf /tmp/oss-cad-suite.tgz -C /usr/local && \
    rm /tmp/oss-cad-suite.tgz

# Set the PATH environment variable for OSS CAD Suite
ENV PATH="/usr/local/oss-cad-suite/bin:${PATH}"

# Clone the project repository
RUN git clone https://github.com/adwait/asplos24-micro-update-lifting.git /project

# Change to the project directory
WORKDIR /project

# Run synthesis scripts
RUN python3 cva6_wbuffer_script.py && \
    python3 cva6_tlb_script.py && \
    python3 cva6_su_script.py && \
    python3 cva6_lsu_script_hier.py && \
    python3 cva6_lsu_script_mono.py

# Run security analysis scripts
RUN python3 cva6_security_script.py
RUN python3 sodor5_security_script.py

# Command to keep the container running
CMD ["/bin/bash"]

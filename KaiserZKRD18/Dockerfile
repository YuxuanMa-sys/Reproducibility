# Use Ubuntu 22.04 as the base image for a more recent environment
FROM ubuntu:22.04

# Set environment variables to prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Install dependencies
RUN apt-get update && apt-get upgrade && apt-get install -y \
    tzdata \
    make \
    m4 \
    unzip \
    curl \
    git \
    sudo \
    libssl-dev \
    ca-certificates \
    bubblewrap \
    aspcud \
    ocaml \
    ocaml-native-compilers \
    pkg-config \
    gcc \
    g++ \
    opam \
    libgmp-dev \
    && apt-get clean

RUN opam init --disable-sandboxing -y && \
    eval $(opam env) && \
    opam switch create 4.10.0

RUN eval $(opam env) && \
    which ocaml && \
    ocaml -version

# Install Coq
RUN eval $(opam env) && \
    opam install -y coq.8.7.2 --update-invariant

# Install UniCoq
RUN eval $(opam env) && \
    opam repo add coq-released https://coq.inria.fr/opam/released && \
    opam install -y coq-unicoq

# Clone the Mtac2 repository
RUN git clone https://github.com/Mtac2/Mtac2.git
WORKDIR Mtac2

# Build Mtac2
RUN eval $(opam env) && \
    coq_makefile -f _CoqProject -o Makefile && \
    make

# Optionally install Mtac2
# RUN make install

# # Set up Coq paths
# RUN echo 'Add LoadPath "/home/opamuser/Mtac2/theories" as Mtac2.' >> ~/.coqrc && \
#     echo 'Add ML Path "/home/opamuser/Mtac2/src".' >> ~/.coqrc

# # Entry point
# CMD ["coqtop"]

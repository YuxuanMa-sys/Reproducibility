# Use the ocaml/opam base image for Ubuntu 16.04
FROM ocaml/opam:ubuntu-16.04

# Update system packages and install additional dependencies
RUN sudo apt-get update && sudo apt-get install -y \
    libboost-all-dev \
    cmake \
    m4 \
    pkg-config \
    scala \
    coq \
    curl \
    ca-certificates \
    libgnutls30 \
    && sudo update-ca-certificates

# Switch to OPAM environment and install OCaml 4.05.0
RUN opam switch create 4.05.0 && \
    eval $(opam env) && \
    opam install -y dune ocamlfind ocamlbuild odoc

# Clone the crellvm repository
WORKDIR /crellvm
RUN git clone https://github.com/snu-sf/crellvm.git .

# Ensure the OPAM repository remote is using HTTPS
RUN git -C /home/opam/opam-repository remote set-url origin https://github.com/ocaml/opam-repository.git

# Clear OPAM cache and update repositories
RUN git -C /home/opam/opam-repository pull origin master && opam update

# Install dependencies and build the project
RUN make init && make llvm && make opt && make test-init




# This runs the benchmark and it's working at the begining, but the benchmark is too big and soonthe docker runs out of space.
RUN make test

# This is working when I ran docker run and manually ran those commands, but somehow failed when I added all these to the dockerfile and build it.
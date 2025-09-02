# Use the specified OCaml OPAM base image with Ubuntu 22.04 and OCaml 4.10
FROM ocaml/opam:ubuntu-22.04-ocaml-4.10

# Install additional system dependencies
RUN sudo apt-get update && \
    sudo apt-get install -y \
    findutils \
    libgmp-dev \
    m4 \
    perl \
    pkg-config \
    zlib1g-dev \
    z3 \
    && sudo apt-get clean

# Set environment variables
ENV RMEM_ROOT=/home/opam/rmem

# Switch to the OPAM user (default in this base image)
USER opam
WORKDIR /home/opam

# Ensure OCaml 4.10 is set in OPAM
RUN opam switch create 4.10.0 || opam switch 4.10.0
RUN eval $(opam env)

# Install ocamlbuild, ocamlfind, and other necessary tools
RUN opam install -y ocamlbuild ocamlfind dune

# Add the REMS opam repository and update OPAM
RUN opam repository add rems https://github.com/rems-project/opam-repository.git#opam2 && \
    opam update

# Clone the rmem repository
RUN git clone https://github.com/rems-project/rmem.git $RMEM_ROOT

# Install the rmem tool with all dependencies
WORKDIR $RMEM_ROOT
RUN opam install . --deps-only -y

#ENV OCAMLBUILD="$(opam var bin)/ocamlbuild"
#ENV PATH="$OCAMLBUILD:$PATH"
## Build rmem using make with optimized mode
# Build rmem using make with optimized mode, specifying OCAMLBUILD path
RUN eval $(opam env) && make MODE=opt OCAMLBUILD=$(which ocamlbuild)


#
## Set PATH to include the rmem executable
#ENV PATH="$RMEM_ROOT:$PATH"
#
## Default command
#CMD ["/bin/bash"]

# Use the specified OCaml OPAM base image
FROM ocaml/opam:ubuntu-22.04-ocaml-5.1

# Install additional system dependencies
RUN sudo apt-get update && \
    sudo apt-get install -y \
    cmake \
    m4 \
    pkg-config \
    libblas-dev \
    liblapack-dev \
    libmpfr-dev \
    libgmp-dev \
    libglpk-dev \
    libffi-dev \
    zlib1g-dev \
    curl \
    python3 \
    && sudo apt-get clean

# Set environment variables
ENV LLVM_SRC_ROOT=/opt/llvm-src
ENV LLVM_BUILD_ROOT=/opt/llvm-build
ENV COAR_ROOT=/home/opam/coar

# Switch to the OPAM user (default in this base image)
USER opam
WORKDIR /home/opam

# Clone the CoAR repository
RUN git clone https://github.com/hiroshi-unno/coar.git $COAR_ROOT

# Install required OCaml packages
WORKDIR $COAR_ROOT
RUN opam install . --deps-only -y

# Install additional OCaml dependencies
RUN opam install -y \
    core \
    core_unix \
    domainslib \
    menhir \
    ppx_deriving_yojson \
    ocaml-compiler-libs \
    ocamlgraph \
    zarith \
    z3 \
    minisat \
    camlzip \
    dune



# Build CoAR
WORKDIR $COAR_ROOT
RUN eval $(opam env) && dune build main.exe

# Set PATH to include CoAR executable
ENV PATH="$COAR_ROOT/_build/default:$PATH"

# Run functional tests
# Example of running Predicate Constraint Satisfiability Checking test
RUN eval $(opam env) && dune exec main -- -c ./config/solver/dbg_pcsat_tbq_ar.json -p pcsp ./benchmarks/CHC/simple/sum.smt2



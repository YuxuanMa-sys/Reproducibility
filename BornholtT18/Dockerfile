# Use Ubuntu as the base image
FROM ubuntu:20.04

# Set environment variables to avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Install required dependencies without build-essential
RUN apt-get update && apt-get install -y \
    curl \
    git \
    libfontconfig1 \
    libcairo2 \
    libcairo2-dev \
    libglib2.0-dev \
    libpango1.0-dev \
    libpixman-1-dev \
    libffi-dev \
    libjpeg62 \
    libjpeg62-dev \
    libgtk2.0-0 \
    libgtk2.0-dev \
    xvfb \
    && apt-get clean


# Download and install Racket 8.14
RUN curl -L https://download.racket-lang.org/installers/8.14/racket-8.14-x86_64-linux-cs.sh -o /tmp/racket.sh && \
    bash /tmp/racket.sh --in-place --dest /usr/racket && \
    rm /tmp/racket.sh

# Add Racket to PATH
ENV PATH="/usr/racket/bin:${PATH}"

# Clone the Rosette repository
RUN git clone https://github.com/emina/rosette.git /usr/src/rosette

# Set working directory to Rosette
WORKDIR /usr/src/rosette

# Install Rosette with verbose output for debugging
RUN raco pkg install --auto

# Run the Demo

WORKDIR /usr/src/rosette/sdsl/fsm

RUN raco make demo.rkt && xvfb-run racket demo.rkt

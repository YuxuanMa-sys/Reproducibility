# Use an official Anaconda image as the base
FROM continuumio/anaconda3:latest

# Set the working directory
WORKDIR /workspace

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Clone the DPMLBench repository
RUN git clone https://github.com/DmsKinson/DPMLBench.git

# Set the working directory to the cloned repo
WORKDIR /workspace/DPMLBench

# Create and activate the conda environment
RUN conda create --name dpml python=3.8 -y && \
    echo "source activate dpml" > ~/.bashrc && \
    /bin/bash -c "source ~/.bashrc"

# Install PyTorch and other dependencies
RUN conda install -n dpml pytorch=1.10.0 torchvision cudatoolkit=11.7 -c pytorch -c nvidia -y

# Activate the conda environment and install the required Python packages
RUN /bin/bash -c "source activate dpml && pip install -r requirements.txt && pip install matplotlib"

# Run the script to replicate plots
RUN /bin/bash -c "source activate dpml && bash replicate_figure.sh"



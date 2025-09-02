FROM ubuntu:14.04

ENV DEBIAN_FRONTEND=noninteractive

# Update packages and install g++
RUN apt-get update && apt-get install -y \
    g++ \
    gcc \
    make \
    wget \
    tar \
    && apt-get clean

# Download the ALAE binary tarball
RUN wget http://hongleiliu.weebly.com/uploads/8/2/0/9/8209923/alae-x64-linux.tar.gz \
    && tar -xzf alae-x64-linux.tar.gz


# Set working directory to the extracted folder (adjust this if the folder name is different)
WORKDIR /ALAE-64bits
#
## Download and extract the example data
RUN wget http://hongleiliu.weebly.com/uploads/8/2/0/9/8209923/example_data_alae.tar.gz \
    && tar -xzf example_data_alae.tar.gz



# Run the custom make script
RUN make
#
WORKDIR /ALAE-64bits/"example data"
#
## Run the bwt_build command using text10M.txt from the example data folder
RUN ../bwt_build text10M.txt text10M.index
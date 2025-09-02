# Use an official Ubuntu base image
FROM ubuntu:20.04

# Set environment variables to avoid user prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package lists and install necessary system dependencies
RUN apt-get update && apt-get install -y \
    gnupg \
    software-properties-common \
    wget \
    python3 \
    python3-pip \
    git \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev \
    gcc \
    make \
    && rm -rf /var/lib/apt/lists/*

# Add the CRAN repository for older R versions
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 'E298A3A825C0D65DFD57CBB651716619E084DAB9' && \
    add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'

# Install the latest R version
RUN apt-get update && apt-get install -y r-base

# Install required R packages
RUN R -e "install.packages(c('rpart', 'e1071', 'CORElearn', 'glmnet'), repos='https://cloud.r-project.org/')"

WORKDIR /app

# Clone the repository
RUN git clone https://github.com/pvn25/Hamlet_Extension.git /app/Hamlet_Extension

# Download the dataset
RUN wget http://cseweb.ucsd.edu/~arunkk/hamlet/RealWorldDatasets.zip -O /tmp/RealWorldDatasets.zip

# Unzip the dataset into the appropriate folder
RUN unzip /tmp/RealWorldDatasets.zip -d /tmp/

# Copy the "Expedia" dataset into the appropriate folder
RUN mkdir -p /app/Hamlet_Extension/Code/Materialize_Joins/Expedia && \
    cp -r /tmp/RealWorldDatasets/Expedia/* /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/

# Rename the required files inside the "Expedia" dataset
RUN mv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/S_listings.csv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/listingsnew.csv && \
    mv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/R2_searches.csv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/searchesnew.csv && \
    mv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/R1_hotels.csv /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/hotelsnew.csv

# Set the working directory to where the R script is located
WORKDIR /app/Hamlet_Extension/Code/Materialize_Joins/Expedia/

# Expose any necessary ports (if applicable)
EXPOSE 8080

# Run the R script when the container starts
RUN Rscript test.R

# Use Ubuntu 14.04 as the base image
FROM ubuntu:14.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=$PATH:/usr/share/sbt/bin

# Install required dependencies
RUN apt-get update && \
    apt-get install -y \
    openjdk-7-jdk \
    wget \
    unzip \
    scala \
    --no-install-recommends && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Download and install sbt from the official site
RUN wget https://github.com/sbt/sbt/releases/download/v0.13.17/sbt-0.13.17.zip -O /tmp/sbt.zip && \
    unzip /tmp/sbt.zip -d /usr/share && \
    rm /tmp/sbt.zip

# Set the working directory
WORKDIR /home/ChemistrySet

# Copy the ChemistrySet project files
COPY . .

# output the log showing the test is successful
RUN sbt -batch -no-colors 'set logLevel := Level.Info' compile test

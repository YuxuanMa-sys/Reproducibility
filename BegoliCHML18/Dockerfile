# Use Ubuntu as the base image
FROM ubuntu:20.04

# Set environment variables to skip tzdata configuration prompts
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York

# Set the working directory inside the container
WORKDIR /app

# Install dependencies: Java, Git, and any required utilities
RUN apt-get update && \
    apt-get install -y openjdk-11-jdk git curl tzdata && \
    rm -rf /var/lib/apt/lists/*

# Set the environment variable for Java
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64
ENV PATH $JAVA_HOME/bin:$PATH

# Clone the Calcite repository (or the project you want to reproduce)
RUN git clone https://github.com/apache/calcite.git

# Change directory to the cloned Calcite repo
WORKDIR /app/calcite

# Run the build process using Gradle
RUN ./gradlew build

# Run functional tests
RUN ./gradlew test




# Use a more recent base image (Ubuntu 20.04 LTS)
FROM ubuntu:18.04


# Set environment variables for non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package lists and install required dependencies
RUN apt-get update && apt-get install -y \
    sudo \
    git \
    wget \
    gnupg2 \
    make \
    software-properties-common \
    apt-transport-https \
    ca-certificates

# Install OpenJDK (>=8)
RUN apt-get update && apt-get install -y openjdk-11-jdk

# Install SBT using wget for GPG key
RUN sudo apt-get update && \
    sudo apt-get install apt-transport-https curl gnupg -yqq && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | sudo tee /etc/apt/sources.list.d/sbt.list && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | sudo tee /etc/apt/sources.list.d/sbt_old.list && \
    curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo -H gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/scalasbt-release.gpg --import && \
    sudo chmod 644 /etc/apt/trusted.gpg.d/scalasbt-release.gpg && \
    sudo apt-get update && \
    sudo apt-get install sbt

# Set environment variables for Java and SBT
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV PATH=$PATH:/usr/lib/jvm/java-11-openjdk-amd64/bin


# RUN sbt sbtVersion

# Acquire code from the internet
RUN git clone https://github.com/Kraks/RefuncAAM.git code && cd code && sbt compile && sbt test

WORKDIR /code

# Install TeX Live for LaTeX tools (including pdflatex), this will be used in generating the paper
RUN apt-get update && apt-get install -y \
    texlive-latex-base \
    texlive-latex-recommended \
    texlive-fonts-recommended \
    texlive-latex-extra \
    texlive-fonts-extra \
    && apt-get clean

# Additional section to generate the paper
WORKDIR /code/paper
RUN make

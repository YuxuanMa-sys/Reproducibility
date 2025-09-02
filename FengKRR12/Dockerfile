
FROM ubuntu:20.04

# Set environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install dependencies
RUN apt-get update && apt-get install -y \
    postgresql-12 \
    postgresql-server-dev-12 \
    gcc \
    python3 \
    python3-pip \
    python3-dev \
    libpq-dev \
    make \
    rsync \
    wget \
    python3-psycopg2 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /usr/src/app

# Download and extract PostgreSQL 12.20 source code
RUN wget https://ftp.postgresql.org/pub/source/v12.20/postgresql-12.20.tar.gz -O postgresql-12.20.tar.gz && \
    tar -xvzf postgresql-12.20.tar.gz && \
    cd postgresql-12.20/src/include && \
    cp -r * /usr/include/postgresql/ && \
    cd /usr/src/app

# copy files from /usr/include/postgresql/12/server/ to /usr/include/postgresql/  those missing headers
RUN rsync -a --ignore-existing /usr/include/postgresql/12/server/ /usr/include/postgresql/
# Download Bismarck project from the provided link
RUN wget http://i.stanford.edu/hazy/victor/downloads/bismarck.tar.gz -O bismarck.tar.gz

# Extract the downloaded archive
RUN tar -xvzf bismarck.tar.gz --strip 1

# Add the include directory to the Makefile in subdirectories of src/ports/postgres/
RUN find src/ports/postgres/ -type f -name Makefile -exec sed -i '/^[[:space:]]*CFLAGS[[:space:]]*=/ s|$| -I/usr/include/postgresql|' {} \;




#WORKDIR /usr/src/app
RUN make

RUN chown -R postgres:postgres /var/lib/postgresql
EXPOSE 5432
#
## Command to start PostgreSQL and run the project
CMD service postgresql start && bash

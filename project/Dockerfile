FROM ubuntu:20.04

# Setting the timezone, such that the selection does not have to 
# be made when installing colcon-common-extensions
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime


# Install necessary build tools
RUN apt-get update && apt-get install -y \
    cmake \
    g++ \ 
    python3-pip \
    git \
    libasio-dev \
    libtinyxml2-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*
    
# Create the workspace
RUN mkdir -p fast_dds


COPY . /fast_dds

# Set working directory
WORKDIR /fast_dds
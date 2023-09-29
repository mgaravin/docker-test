FROM ubuntu:20.04

SHELL ["/bin/bash", "-c"]

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Installing dependent packages required for DPDK Build
RUN apt-get update

RUN apt install -y apt-utils \
    git \
    libtool \
    clang \
    gcc \
    g++ \
    autoconf \
    automake \
    autoconf-archive \
    libconfig++-dev \
    libgc-dev \
    unifdef \
    libffi-dev \
    libboost-iostreams-dev \
    libboost-graph-dev \
    llvm \
    pkg-config \
    flex libfl-dev \
    zlib1g-dev \
    iproute2 \
    net-tools \
    iputils-arping \
    iputils-ping \
    iputils-tracepath \
    python \
    bison \
    python3-setuptools \
    python3-pip \
    python3-wheel \
    python3-cffi \
    libedit-dev \
    libgmp-dev \
    libexpat1-dev \
    libboost-dev \
    google-perftools \
    curl \
    connect-proxy \
    coreutils \
    vim \
    sudo
RUN apt -y install pip
# Installing all PYTHON packages
RUN python3 -m pip install --no-cache-dir --upgrade pip && \
    python3 -m pip install --no-cache-dir grpcio && \
    python3 -m pip install --no-cache-dir ovspy \
    protobuf \
    p4runtime \
    pyelftools \
    scapy \
    six \
    cmake>=3.15.0 \
    meson==0.59.4 \
    ninja>=1.8.2
# Repo tool    
RUN cd /usr/local/src && git clone https://gerrit.googlesource.com/git-repo && cd git-repo && git checkout repo-1
RUN ln -s /usr/local/src/git-repo/repo /usr/bin/repo
RUN apt purge cmake -y || true
RUN rm -rf /usr/local/bin/cmake || true && rm -rf ~/.local/bin/cmake || true
RUN wget https://cmake.org/files/v3.20/cmake-3.20.2-linux-x86_64.sh -O /tmp/cmake-3.20.2-linux-x86_64.sh && chmod a+x /tmp/cmake-3.20.2-linux-x86_64.sh && mkdir -p /opt/rh/cmake
RUN sh /tmp/cmake-3.20.2-linux-x86_64.sh --prefix=/opt/rh/cmake --skip-license
RUN ln -s /opt/rh/cmake/bin/cmake /usr/local/bin/cmake && cmake --version
RUN git clone https://github.com/p4lang/p4-dpdk-target --recursive /opt/p4-dpdk-target
RUN mkdir -p /opt/p4sde/install && cd /opt/p4-dpdk-target && ./autogen.sh && ./configure --prefix=/opt/p4sde/install && make && make install
RUN cd /opt/ && rm -rf p4-dpdk-target


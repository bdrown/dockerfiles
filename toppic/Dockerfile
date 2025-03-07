FROM ubuntu:22.04
LABEL maintainer "Bryon Drown <bsdrown@purdue.edu>"

ARG DEBIAN_FRONTEND=noninteractive
ENV TOPPIC_VERSION=v1.7.9

RUN apt-get -y update --fix-missing \
    && apt-get -y upgrade \
    && apt-get -y update \
    && apt-get -y autoremove
	
RUN apt-get -y update \
    && apt-get -y install \
       build-essential cmake \
    && apt-get -y install \
       zlib1g-dev \
       libboost-filesystem-dev \
       libboost-program-options-dev \
       libboost-system-dev \
       libboost-thread-dev \
       libboost-iostreams-dev \
       libboost-chrono-dev \
       libxerces-c-dev	\
       git \
    && apt-get -y install qtbase5-dev
		
RUN git clone --depth 1 --branch ${TOPPIC_VERSION} https://github.com/toppic-suite/toppic-suite.git

RUN mkdir -p toppic-suite/build \
    && cd toppic-suite/build \
    && cmake .. \
    && make \
    && mv ../resources ../bin \
    && chmod -R 777 /toppic-suite/bin

ENV LANG=en_US.UTF-8 LANGUAGE=en_US.UTF-8 LC_ALL=C PATH=/toppic-suite/bin:$PATH

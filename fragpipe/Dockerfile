FROM ubuntu:24.04 AS base
LABEL maintainer="Bryon Drown <bsdrown@purdue.edu>"

ARG DEBIAN_FRONTEND=noninteractive
ENV FRAGPIPE_VERSION=23.0

# update and upgrade packages
RUN apt-get -y update --fix-missing \
    && apt-get -y upgrade \
    && apt-get -y update \
    && apt-get -y autoremove

# install mono
RUN apt-get -y install ca-certificates gnupg
RUN gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mono-official-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb [signed-by=/usr/share/keyrings/mono-official-archive-keyring.gpg] https://download.mono-project.com/repo/ubuntu stable-focal main" | tee /etc/apt/sources.list.d/mono-official-stable.list
RUN apt-get -y update
RUN apt-get -y install mono-devel

RUN apt install -y software-properties-common
RUN add-apt-repository ppa:deadsnakes/ppa
RUN add-apt-repository ppa:dotnet/backports
RUN apt-get -y update

# install dependencies
RUN apt-get -y install \
    git \
    python3.11 \
    python3-pip \
    tar \
    unzip \
    wget \
    openjdk-17-jdk \
    vim \
    dotnet-runtime-6.0

# install python packages
RUN pip uninstall --break-system-packages easypqp \
    && pip install --break-system-packages git+https://github.com/Nesvilab/easypqp.git@master \
    && pip install --break-system-packages lxml \
    && pip install --break-system-packages plotly \
    && pip install --break-system-packages kaleido \
    && pip install --break-system-packages narwhals \
    && pip install --break-system-packages pyarrow \
    && pip install --break-system-packages pypdf2

# create a directory with 777 permission and set it to the work directory
RUN mkdir /fragpipe_bin
RUN chmod 777 /fragpipe_bin
WORKDIR /fragpipe_bin

# create directories
RUN mkdir tmp
RUN chmod 777 tmp

# download and install fragPipe
RUN wget https://github.com/Nesvilab/FragPipe/releases/download/${FRAGPIPE_VERSION}/FragPipe-${FRAGPIPE_VERSION}-linux.zip -P fragPipe-${FRAGPIPE_VERSION}
RUN unzip fragPipe-${FRAGPIPE_VERSION}/FragPipe-${FRAGPIPE_VERSION}-linux.zip -d fragPipe-${FRAGPIPE_VERSION}
RUN rm fragPipe-${FRAGPIPE_VERSION}/FragPipe-${FRAGPIPE_VERSION}-linux.zip
RUN chmod -R 777 /fragpipe_bin

# set environment variables
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64/
RUN export JAVA_HOME

FROM base AS complete

ENV FRAGPIPE_VERSION=23.0 MSF_VERSION=4.2 IONQUANT_VERSION=1.11.9 DIATRACER_VERSION=1.2.5
WORKDIR /fragpipe_bin

# to include MSFragger and other tools into the image, they need to be accessed from the host
ADD MSFragger-${MSF_VERSION}.zip diaTracer-${DIATRACER_VERSION}.zip IonQuant-${IONQUANT_VERSION}.zip /fragpipe_bin/

RUN unzip MSFragger-${MSF_VERSION}.zip -d fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/tools \
    && unzip diaTracer-${DIATRACER_VERSION}.zip -d fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/tools \
    && unzip IonQuant-${IONQUANT_VERSION}.zip -d fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/tools
RUN rm /fragpipe_bin/*.zip

# Apptainer uses read-only file systems so we need to provide a starting point for the cache at buildtime
RUN mkdir -p /fragpipe_bin/fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/cache \
    && echo "fragpipe-config.tools-folder=/fragpipe_bin/fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/tools" >> /fragpipe_bin/fragPipe-${FRAGPIPE_VERSION}/fragpipe-${FRAGPIPE_VERSION}/cache/fragpipe-ui.cache

ENV LANG=en_US.UTF-8 LANGUAGE=en_US.UTF-8 LC_ALL=C

# set entrypoint
ENTRYPOINT [ "/fragpipe_bin/fragPipe-23.0/fragpipe-23.0/bin/fragpipe" ]
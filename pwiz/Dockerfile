FROM ubuntu:22.04 AS base
LABEL maintainer="Bryon Drown <bsdrown@purdue.edu>"

ARG DEBIAN_FRONTEND=noninteractive

# update and upgrade packages
RUN apt-get -y update --fix-missing \
    && apt-get -y upgrade \
    && apt-get -y update \
    && apt-get -y autoremove

RUN apt-get install -y \
    wget \
    gnupg

RUN mkdir -pm755 /etc/apt/keyrings \
    && wget -O - https://dl.winehq.org/wine-builds/winehq.key | gpg --dearmor -o /etc/apt/keyrings/winehq-archive.key -

RUN dpkg --add-architecture i386 \
    && wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/jammy/winehq-jammy.sources \
    && apt-get -y update

RUN apt-get install -y --install-recommends \
    winehq-stable \
    winetricks

ENV WINEARCH=win32 WINEPREFIX=~/.wine

RUN winetricks vcrun2008

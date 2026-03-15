FROM ubuntu:22.04
LABEL maintainer="Bryon Drown <bsdrown@purdue.edu>"

ARG DEBIAN_FRONTEND=noninteractive
ENV CHIMERAX_VERSION=1.9

RUN apt-get update

COPY ucsf-chimerax_${CHIMERAX_VERSION}ubuntu22.04_amd64.deb /
RUN apt-get install -y /ucsf-chimerax_${CHIMERAX_VERSION}ubuntu22.04_amd64.deb
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm /ucsf-chimerax_${CHIMERAX_VERSION}ubuntu22.04_amd64.deb

ENTRYPOINT [ "/usr/bin/chimerax" ]
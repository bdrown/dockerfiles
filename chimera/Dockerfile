FROM ubuntu:22.04
LABEL maintainer="Bryon Drown <bsdrown@purdue.edu>"

ARG DEBIAN_FRONTEND=noninteractive
ENV CHIMERA_VERSION=1.19

# Install dependencies
RUN apt-get update && apt-get install -y \
    libfreetype-dev \
    libglu1-mesa-dev \
    libxft2 \
    libxss1

# Chimera installer downloaded from https://www.cgl.ucsf.edu/chimera/download.html
COPY chimera-${CHIMERA_VERSION}-linux_x86_64.bin /

RUN chmod +x /chimera-${CHIMERA_VERSION}-linux_x86_64.bin

# Responses to interactive prompts install at /chimera and make sym link in /usr/local/bin
RUN echo "/chimera\n^H^H^Hno\n2\n\n" | /chimera-${CHIMERA_VERSION}-linux_x86_64.bin
RUN rm /chimera-${CHIMERA_VERSION}-linux_x86_64.bin

ENTRYPOINT [ "/usr/local/bin/chimera64-1.19" ]
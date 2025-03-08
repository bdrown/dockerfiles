FROM ubuntu:22.04

LABEL software="diann"
LABEL software.version="2.0.2"
LABEL about.summary="DIA-NN - a universal software for data-independent acquisition (DIA) proteomics data processing."
LABEL about.home="https://github.com/vdemichev/DiaNN"
LABEL about.documentation="https://github.com/vdemichev/DiaNN"
LABEL about.license_file="https://github.com/vdemichev/DiaNN/releases/download/2.0/LICENSE.txt"
LABEL about.tags="Proteomics"

ENV DEBIAN_FRONTEND=noninteractive
ENV DIANN_VERSION=2.0.2

# Update package lists and ensure package versions are up to date
RUN apt-get update && apt-get upgrade -y

# Install necessary packages including locales
RUN apt-get install wget unzip libgomp1 locales -y

# Configure locale to avoid runtime errors
RUN locale-gen en_US.UTF-8 && \
    update-locale LANG=en_US.UTF-8

# Set environment variables for locale
ENV LANG=en_US.UTF-8 LANGUAGE=en_US:en LC_ALL=en_US.UTF-8

# Download DIA-NN version <version>
ADD https://github.com/vdemichev/DiaNN/releases/download/2.0/DIA-NN-${DIANN_VERSION}-Academia-Linux.zip diann-${DIANN_VERSION}.Linux.zip

# Unzip the DIA-NN package
RUN unzip diann-${DIANN_VERSION}.Linux.zip \
    && rm diann-${DIANN_VERSION}.Linux.zip

# Set appropriate permissions for the DIA-NN folder
RUN chmod -R 775 /diann-${DIANN_VERSION}

ENTRYPOINT [ "/diann-2.0.2/diann-linux" ]

# NOTE: It is entirely the user's responsibility to ensure compliance with DIA-NN license terms.
# Please review the licensing terms for DIA-NN before using or distributing this Docker image.
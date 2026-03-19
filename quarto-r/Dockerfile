FROM rocker/verse:4.4
RUN apt-get update && apt-get install -y curl && \
    curl -fsSL https://github.com/quarto-dev/quarto-cli/releases/download/v1.8.27/quarto-1.8.27-linux-amd64.deb -o quarto.deb && \
    dpkg -i quarto.deb && rm quarto.deb

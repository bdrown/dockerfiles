FROM rocker/verse:4.4
RUN apt-get update && apt-get install -y curl && \
    curl -fsSL https://quarto.org/download/1.8.27/quarto-linux-amd64.deb -o quarto.deb && \
    dpkg -i quarto.deb && rm quarto.deb

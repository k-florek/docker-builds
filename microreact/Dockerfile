FROM node:20.11.1-slim AS app

ARG SOFTWARENAME_VER="v265.0.0"

LABEL base.image="node:20.11.1-slim"
LABEL dockerfile.version="1"
LABEL software="Microreact"
LABEL software.version="${SOFTWARENAME_VER}"
LABEL description="Open data visualization and sharing for genomic epidemiology."
LABEL website="https://github.com/microreact/server"
LABEL license="https://github.com/microreact/server/blob/main/LICENSE"
LABEL maintainer="Kelsey Florek"
LABEL maintainer.email="kelsey.florek@slh.wisc.edu"

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
 wget \
 ca-certificates \
 git && \
 apt-get autoclean && rm -rf /var/lib/apt/lists/*

# Clone microreact from github and build/install
RUN git config --global url."https://github.com/".insteadOf git@github.com: && \
 git config --global url."https://".insteadOf git:// && \
 mkdir /microreact && \
 cd /microreact && \
 git clone --branch ${SOFTWARENAME_VER} https://github.com/microreact/server.git && \
 cd server && \
 git submodule update --init && \
 npm install  && \
 npm run build

# Copy default configuration file
RUN mkdir /microreact/config && \
 cp /microreact/server/defaults.json /microreact/config/config.json

# set environment parameter for configuration file
ENV CONFIG_FILE=/microreact/config/config.json \
 LC_ALL=C

# Command to start microreact server
ENTRYPOINT [ "npm", "run", "start"]

# 'WORKDIR' sets working directory
WORKDIR /microreact/server

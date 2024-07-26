FROM --platform=linux/amd64 node:bookworm-slim as node
FROM --platform=linux/amd64 debian:bookworm-slim

ARG user

COPY --from=node /usr/local/bin /usr/local/bin
COPY --from=node /usr/local/lib/node_modules/npm /usr/local/lib/node_modules/npm

RUN apt-get update && \
    apt-get install -y --purge --no-install-recommends \
    curl \
    ca-certificates \
    gpg \
    sudo \
    wget \
    git \
    openssh-client && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    install -dm 755 /etc/apt/keyrings

RUN useradd -m -s /bin/bash ${user} && \
    echo "${user}\tALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/users && \
    chmod 0440 /etc/sudoers.d/users && \
    visudo -c

RUN npm i -g bun

RUN curl -LO https://github.com/neovim/neovim/releases/download/v0.10.1/nvim-linux64.tar.gz && \
    tar xzvf nvim-linux64.tar.gz && \
    sudo ln -s /nvim-linux64/bin/nvim /usr/local/bin/nvim
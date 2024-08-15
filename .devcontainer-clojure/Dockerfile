FROM clojure:temurin-21-lein-2.11.2-bookworm-slim

ARG user

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

USER $user

RUN cd /home/${user} && \
    curl -LO https://github.com/neovim/neovim/releases/download/v0.10.1/nvim-linux64.tar.gz && \
    tar xzvf nvim-linux64.tar.gz && \
    sudo ln -s /home/${user}/nvim-linux64/bin/nvim /usr/local/bin/nvim

## arm64 nvim build setting
# RUN cd /home/${user} && \
#     sudo apt update && apt install -y --no-install-recommends \
#     ninja-build gettext cmake unzip build-essential ca-certificates
#
# RUN git clone https://github.com/neovim/neovim.git && \
#     cd neovim && \
#     git checkout stable && \
#     make CMAKE_BUILD_TYPE=RelWithDebInfo && \
#     sudo make install

FROM haskell:slim-bullseye

ARG user

ARG GHC_VERSION=9.4.8 \
    CABAL_VERSION=3.8.1.0 \
    HLS_VERSION=2.7.0.0

ENV PATH="${PATH}:/root/.ghcup/bin:/root/.cabal/bin:/root/.ghc/bin" \
    BOOTSTRAP_HASKELL_NONINTERACTIVE=1 \
    BOOTSTRAP_HASKELL_GHC_VERSION=${GHC_VERSION} \
    BOOTSTRAP_HASKELL_CABAL_VERSION=${CABAL_VERSION} \
    BOOTSTRAP_HASKELL_INSTALL_HLS=${HLS_VERSION}

RUN apt-get update && \
    apt-get install -y --purge --no-install-recommends \
    curl \
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

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh \
    cabal update && \
    stack config set install-ghc false --global && \
    stack config set system-ghc true --global

RUN curl -LO https://github.com/neovim/neovim/releases/download/v0.10.1/nvim-linux64.tar.gz && \
    tar xzvf nvim-linux64.tar.gz && \
    sudo ln -s /nvim-linux64/bin/nvim /usr/local/bin/nvim

## arm64 nvim build setting
# RUN sudo apt update && apt install -y --no-install-recommends \
#     ninja-build gettext cmake unzip build-essential ca-certificates
#
# RUN git clone https://github.com/neovim/neovim.git && \
#     cd neovim && \
#     git checkout stable && \
#     make CMAKE_BUILD_TYPE=RelWithDebInfo && \
#     sudo make install

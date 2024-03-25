# Use a lightweight Debian-based image as the base
FROM debian:latest

# Install essential development tools
RUN apt-get update && apt-get install -y \
        git \
        curl \
        build-essential \
        && rm -rf /var/lib/apt/lists/*

# Upgrade Neovim
RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz && \
    rm -rf /opt/nvim && \
    tar -C /opt -xzf nvim-linux64.tar.gz
ENV PATH=$PATH:/opt/nvim-linux64/bin

# Install Go
ARG GO_VERSION=1.22.1
RUN curl -L https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz \
        --output go${GO_VERSION}.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go${GO_VERSION}.linux-amd64.tar.gz && \
    rm go${GO_VERSION}.linux-amd64.tar.gz

# Set Go environment variables
ENV GOPATH=/go
ENV PATH=$PATH:/usr/local/go/bin:$GOPATH/bin

# Create a directory for Go projects
RUN mkdir -p $GOPATH/src $GOPATH/bin

# Add vim plugins
RUN mkdir -p /root/.config/nvim
COPY init.lua /root/.config/nvim/

# Set up a default working directory
WORKDIR $GOPATH/src

# Default command to run when the container starts
CMD ["bash", "-c", "startup.sh"]
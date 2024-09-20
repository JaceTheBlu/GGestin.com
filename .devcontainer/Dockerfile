ARG NODE_VERSION=18
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:${NODE_VERSION}

# HUGO_VARIANT can be either 'hugo' for the standard version or 'hugo_extended' for the extended version.
ARG HUGO_VARIANT=hugo
# HUGO_VERSION can be either 'latest' or a specific version number for Hugo
ARG HUGO_VERSION=latest
# GO_VERSION can be either 'latest' or a specific version number for Go
ARG GO_VERSION=latest

# Download Hugo and Go
RUN apt-get update && apt-get install -y ca-certificates openssl git curl && \
    rm -rf /var/lib/apt/lists/* && \
    # Fetch the latest Hugo version if HUGO_VERSION is set to 'latest'
    case ${HUGO_VERSION} in \
    latest) \
    export HUGO_VERSION=$(curl -s https://api.github.com/repos/gohugoio/hugo/releases/latest | grep "tag_name" | awk '{print substr($2, 3, length($2)-4)}') ;;\
    esac && \
    echo "Installing Hugo version ${HUGO_VERSION}" && \
    # Set the architecture for Hugo and Go
    case $(uname -m) in \
    aarch64) \
    export ARCH=arm64 ;; \
    *) \
    export ARCH=amd64 ;; \
    esac && \
    echo "Detected architecture ${ARCH}" && \
    # Download and install Hugo
    wget -O ${HUGO_VERSION}.tar.gz https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/${HUGO_VARIANT}_${HUGO_VERSION}_Linux-${ARCH}.tar.gz && \
    tar xf ${HUGO_VERSION}.tar.gz && \
    mv hugo /usr/bin/hugo && \
    # Fetch the latest Go version if GO_VERSION is set to 'latest'
    case ${GO_VERSION} in \
    latest) \
    export GO_VERSION=$(curl -s https://go.dev/VERSION?m=text) ;;\
    esac && \
    echo "Installing Go version ${GO_VERSION}" && \
    # Download and install Go
    wget https://go.dev/dl/go${GO_VERSION}.linux-${ARCH}.tar.gz && \
    tar -C /usr/local -xzf go${GO_VERSION}.linux-${ARCH}.tar.gz && \
    rm go${GO_VERSION}.linux-${ARCH}.tar.gz && \
    echo "export PATH=\$PATH:/usr/local/go/bin" >> /etc/profile

# Set the default shell to bash for PATH updates to take effect
SHELL ["/bin/bash", "-c"]

# Hugo dev server port
EXPOSE 1313

# [Optional] Uncomment this section to install additional OS packages you may want.
#
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment if you want to install more global node packages
# RUN sudo -u node npm install -g <your-package-list-here>

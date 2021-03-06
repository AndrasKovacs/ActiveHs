FROM debian:stable-slim

# Install stack dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      netbase \
      curl \
      ca-certificates \
      g++ \
      gcc \
      git \
      gnupg \
      libc6-dev \
      libffi-dev \
      libgmp-dev \
      make \
      xz-utils \
      zlib1g-dev

# Install stack
RUN curl -L https://github.com/commercialhaskell/stack/releases/download/v1.7.1/stack-1.7.1-linux-x86_64.tar.gz -o /usr/local/bin/stack.tar.gz && \
    tar xf /usr/local/bin/stack.tar.gz -C /usr/local/bin --strip-components=1 stack-1.7.1-linux-x86_64/stack && \
    chmod +x /usr/local/bin/stack

# Install extra libraries for ActiveHs dependencies
# libtinfo-dev  for hint
# libpcre3-dev  for pcre-light
# pkg-config    for pcre-light
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      libtinfo-dev \
      libpcre3-dev \
      pkg-config

# Make convenience scripts available
RUN ln -s /development/activehs/docker/container-script/build /development/activehs/docker/container-script/run /usr/local/bin/
    
# Install ActiveHs tool dependencies, locales and useful tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      dvipng \
      graphviz \
      locales \
      screen \
      texlive-latex-recommended \
      texlive-latex-extra

# Set locale         
RUN echo en_US.UTF-8 UTF-8 >> /etc/locale.gen && \
    locale-gen
ENV LC_ALL en_US.UTF-8

# Create user
RUN adduser --system --uid 1000 --group dev
USER dev

# Build ActiveHs dependencies
COPY --chown=dev activehs.cabal stack.yaml /development/init/
RUN stack --stack-yaml=/development/init/stack.yaml setup
RUN stack --stack-yaml=/development/init/stack.yaml --no-system-ghc build --only-dependencies

ENV STACK_YAML=/development/activehs/stack.yaml

VOLUME /development/activehs
VOLUME /activehs-server

WORKDIR /activehs-server

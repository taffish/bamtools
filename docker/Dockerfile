ARG BAMTOOLS_VERSION=2.5.3

FROM debian:12-slim AS builder

ENV DEBIAN_FRONTEND=noninteractive
ARG BAMTOOLS_VERSION

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      ca-certificates \
      cmake \
      curl \
      g++ \
      make \
      zlib1g-dev \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp/bamtools

RUN curl -fsSL "https://github.com/pezmaster31/bamtools/archive/refs/tags/v${BAMTOOLS_VERSION}.tar.gz" \
      -o bamtools.tar.gz \
 && tar -xzf bamtools.tar.gz --strip-components=1 \
 && cmake -S . -B build \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/opt/bamtools \
 && cmake --build build --parallel "$(nproc)" \
 && cmake --install build \
 && /opt/bamtools/bin/bamtools --version 2>&1 | grep -F "bamtools ${BAMTOOLS_VERSION}" >/dev/null

FROM debian:12-slim

ENV DEBIAN_FRONTEND=noninteractive
ARG BAMTOOLS_VERSION

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      zlib1g \
 && rm -rf /var/lib/apt/lists/*

COPY --from=builder /opt/bamtools/bin/bamtools /usr/local/bin/bamtools

RUN bamtools --help >/dev/null 2>&1 \
 && bamtools --version 2>&1 | grep -F "bamtools ${BAMTOOLS_VERSION}" >/dev/null

WORKDIR /root

ENV TAFFISH_ENV=TAFFISH
ENV TAFFISH_NAME=bamtools

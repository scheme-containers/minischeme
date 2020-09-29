# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      gcc make libc-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://raw.githubusercontent.com/lambdaconservatory/scheme-release-archive/master/minischeme/0.85k4/minischeme.tar.gz minischeme.tar.gz
RUN sha256sum minischeme.tar.gz && sha256sum -c checksum
RUN mkdir minischeme && tar -C minischeme --strip-components 1 -xf minischeme.tar.gz
WORKDIR /build/minischeme
RUN mv makefile Makefile
RUN make
RUN mkdir /usr/local/share/miniscm \
 && install -m 755 miniscm  /usr/local/share/miniscm/ \
 && install -m 644 init.scm /usr/local/share/miniscm/

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/
CMD ["scheme-banner"]

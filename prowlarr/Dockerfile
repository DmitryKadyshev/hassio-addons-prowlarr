ARG BUILD_FROM=ghcr.io/hassio-addons/base:18.0.3
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
ARG PROWLARR_VERSION=1.37.0.5076
ARG BUILD_ARCH=amd64
ARG BUILD_VERSION=dev
ENV XDG_CONFIG_HOME="/config/xdg"
# hadolint ignore=DL3003,DL3042
RUN \
  apk add --no-cache \
    icu-libs=76.1-r1 \
    sqlite-libs=3.49.2-r1 \
    xmlstarlet=1.6.1-r2 \
  \
  && if [ "${BUILD_ARCH}" = "aarch64" ]; then PROWLARR_ARCH="arm64"; \
  elif [ "${BUILD_ARCH}" = "amd64" ]; then PROWLARR_ARCH="x64"; \
  elif [ "${BUILD_ARCH}" = "armv7" ]; then PROWLARR_ARCH="arm"; fi \
  \
  && curl -J -L -o /tmp/prowlarr.tar.gz \
      "https://github.com/Prowlarr/Prowlarr/releases/download/v${PROWLARR_VERSION}/Prowlarr.master.${PROWLARR_VERSION}.linux-musl-core-${PROWLARR_ARCH}.tar.gz" \
  && mkdir -p /opt/bin \
  && tar zxvf \
      /tmp/prowlarr.tar.gz \
      --strip 1 -C /opt/bin \
  \
  && echo -e "UpdateMethod=docker\nBranch=master\nPackageVersion=${BUILD_VERSION}\nPackageAuthor=Home Assistant Community Add-ons" \
    > /opt/package_info \
  \
  && rm -f -r \
    /tmp/* \
    /opt/bin/Prowlarr.Update

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

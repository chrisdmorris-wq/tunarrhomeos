# https://developers.home-assistant.io/docs/apps/configuration#app-dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.23
FROM ${BUILD_FROM}

# Install dependencies
RUN apk add --no-cache \
    curl \
    wget \
    jq \
    ffmpeg

# Download and extract Tunarr
WORKDIR /app
RUN LATEST_URL=$(curl -s https://api.github.com/repos/chrisbenincasa/tunarr/releases/latest | jq -r '.assets[] | select(.name | contains("linux-x64")) | .browser_download_url' | head -1) && \
    echo "Downloading Tunarr from: $LATEST_URL" && \
    wget -q "$LATEST_URL" -O tunarr.tar.gz && \
    tar xzf tunarr.tar.gz && \
    rm tunarr.tar.gz && \
    chmod +x /app/tunarr

# Copy app files
COPY rootfs /

# Labels
LABEL \
    org.opencontainers.image.title="Home Assistant App: Tunarr" \
    org.opencontainers.image.description="Create custom live TV channels from Plex, Jellyfin, Emby, or local files" \
    org.opencontainers.image.source="https://github.com/chrisdmorris-wq/tunarrhomeos" \
    org.opencontainers.image.licenses="Apache License 2.0"

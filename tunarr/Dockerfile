ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    ffmpeg \
    wget \
    curl

# Create app directory
WORKDIR /app

# Download Tunarr - try to get the latest release
RUN LATEST_RELEASE=$(curl -s https://api.github.com/repos/chrisbenincasa/tunarr/releases/latest | grep -oP '"browser_download_url": "\K.*linux-x64\.tar\.gz(?=")' | head -1) && \
    if [ -z "$LATEST_RELEASE" ]; then \
      echo "Failed to find latest release, trying npm install..."; \
      npm install -g @chrisbenincasa/tunarr; \
    else \
      echo "Downloading from: $LATEST_RELEASE"; \
      wget -q "$LATEST_RELEASE" -O tunarr-linux.tar.gz && \
      tar xzf tunarr-linux.tar.gz && \
      rm tunarr-linux.tar.gz && \
      chmod +x tunarr || (echo "Direct download failed, using npm" && npm install -g @chrisbenincasa/tunarr); \
    fi

# Create config directory
RUN mkdir -p /config/tunarr && \
    mkdir -p /media_cache

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8000 || exit 1

# Start Tunarr
CMD ["tunarr", "--config", "/config/tunarr"]

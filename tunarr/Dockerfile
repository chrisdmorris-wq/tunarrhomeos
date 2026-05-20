ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    ffmpeg \
    wget \
    curl \
    jq

# Create app directory
WORKDIR /app

# Download Tunarr using npm as the primary method
RUN npm install -g @chrisbenincasa/tunarr || \
    (echo "npm install failed, attempting direct download..." && \
    DOWNLOAD_URL=$(curl -s https://api.github.com/repos/chrisbenincasa/tunarr/releases/latest | jq -r '.assets[] | select(.name | contains("linux-x64")) | .browser_download_url' | head -1) && \
    if [ -n "$DOWNLOAD_URL" ]; then \
      wget -q "$DOWNLOAD_URL" -O tunarr.tar.gz && \
      tar xzf tunarr.tar.gz && \
      rm tunarr.tar.gz; \
    else \
      echo "Failed to download Tunarr"; exit 1; \
    fi)

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

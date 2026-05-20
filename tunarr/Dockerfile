ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install Tunarr dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    ffmpeg \
    wget \
    curl

# Create app directory
WORKDIR /app

# Download latest Tunarr release
RUN DOWNLOAD_URL=$(curl -s https://api.github.com/repos/chrisbenincasa/tunarr/releases/latest | grep -o '"browser_download_url":"[^"]*linux-x64[^"]*"' | grep -o 'https[^"]*' | head -1) && \
    if [ -z "$DOWNLOAD_URL" ]; then \
      echo "Error: Could not find Tunarr download URL"; exit 1; \
    fi && \
    echo "Downloading from: $DOWNLOAD_URL" && \
    wget -q "$DOWNLOAD_URL" -O tunarr.tar.gz && \
    tar xzf tunarr.tar.gz && \
    rm tunarr.tar.gz && \
    chmod +x tunarr

# Create config and cache directories
RUN mkdir -p /config/tunarr && \
    mkdir -p /media_cache

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8000 || exit 1

# Start Tunarr
CMD ["/app/tunarr", "--config", "/config/tunarr"]

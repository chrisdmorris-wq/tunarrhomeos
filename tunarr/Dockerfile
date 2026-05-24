ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install dependencies
RUN apk add --no-cache \
    wget \
    curl \
    jq

# Create app directory
WORKDIR /app

# Download latest Tunarr release using jq for safe JSON parsing
RUN LATEST_URL=$(curl -s https://api.github.com/repos/chrisbenincasa/tunarr/releases/latest | jq -r '.assets[] | select(.name | contains("linux-x64")) | .browser_download_url' | head -1) && \
    echo "Downloading Tunarr from: $LATEST_URL" && \
    wget -q "$LATEST_URL" -O tunarr.tar.gz && \
    tar xzf tunarr.tar.gz && \
    rm tunarr.tar.gz && \
    ls -la /app/ && \
    find /app -name "tunarr" -type f && \
    chmod +x /app/tunarr

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

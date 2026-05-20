ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    ffmpeg \
    wget

# Create app directory
WORKDIR /app

# Download Tunarr binary
RUN wget -q https://github.com/chrisbenincasa/tunarr/releases/download/latest/tunarr-linux-x64.tar.gz && \
    tar xzf tunarr-linux-x64.tar.gz && \
    rm tunarr-linux-x64.tar.gz && \
    chmod +x tunarr

# Create config directory
RUN mkdir -p /config/tunarr && \
    mkdir -p /media_cache

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:8000 || exit 1

# Start Tunarr
CMD ["/app/tunarr", "--config", "/config/tunarr"]

FROM docker.n8n.io/n8nio/n8n:latest
USER root

# Install Chromium and required dependencies for browser automation
RUN apk update && apk add --no-cache \
    chromium \
    nss \
    freetype \
    freetype-dev \
    harfbuzz \
    ca-certificates \
    ttf-freefont \
    ttf-liberation \
    fontconfig

# Install additional system dependencies for browser stability
RUN apk add --no-cache \
    dbus-x11 \
    mesa-gl \
    mesa-dri-gallium \
    udev \
    xvfb \
    libxscrnsaver \
    at-spi2-core

# Create dbus directory and set permissions
RUN mkdir -p /run/dbus && \
    dbus-daemon --system --fork

# Set environment variables for browser (these will be used by n8n-nodes-mindsurf when installed)
ENV PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1
ENV PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium-browser
ENV CHROME_BIN=/usr/bin/chromium-browser
ENV CHROME_PATH=/usr/bin/chromium-browser

# Set display for headless mode
ENV DISPLAY=:99

# Create a startup script to handle dbus and xvfb
RUN echo '#!/bin/sh' > /start.sh && \
    echo 'rm -f /run/dbus/pid 2>/dev/null' >> /start.sh && \
    echo 'dbus-daemon --system --fork 2>/dev/null || true' >> /start.sh && \
    echo 'Xvfb :99 -screen 0 1920x1080x24 -nolisten tcp &' >> /start.sh && \
    echo 'n8n' >> /start.sh && \
    chmod +x /start.sh

USER node

ENTRYPOINT ["/start.sh"]
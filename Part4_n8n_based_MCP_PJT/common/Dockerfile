FROM docker.n8n.io/n8nio/n8n:latest
USER root
# Install Chromium for web automation
RUN apk add --no-cache chromium chromium-chromedriver
# Install Firefox and all required dependencies for PDF generation
RUN apk add --no-cache \
    firefox \
    firefox-esr \
    ttf-liberation \
    fontconfig \
    gcompat \
    libstdc++ \
    dbus \
    dbus-x11 \
    mesa-gl \
    mesa-dri-gallium \
    udev \
    xvfb
# Install Playwright globally
RUN npm install -g playwright-core
# Create symbolic links for Playwright to fidnd browsers
RUN ln -sf /usr/bin/firefox /usr/bin/firefox-stable && \
    ln -sf /usr/bin/chromium-browser /usr/bin/chromium
# Set environment variables
ENV PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1
ENV PLAYWRIGHT_BROWSERS_PATH=/usr/bin
ENV PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium-browser
ENV PLAYWRIGHT_FIREFOX_EXECUTABLE_PATH=/usr/bin/firefox

USER node
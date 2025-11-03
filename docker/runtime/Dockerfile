#
# Agent Runtime - Base execution environment for AI agents
# Ubuntu 22.04 + Node.js 20 + Development tools
#

FROM ubuntu:22.04

LABEL maintainer="Deepractice AI <hello@deepractice.ai>"
LABEL org.opencontainers.image.title="Agent Runtime"
LABEL org.opencontainers.image.description="Base runtime environment for AI agents with Ubuntu, Node.js, and essential tools"
LABEL org.opencontainers.image.version="1.0.0"

ENV DEBIAN_FRONTEND=noninteractive

# Install essential packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      # Network and certificates
      curl wget ca-certificates gnupg \
      # Process and system tools
      sudo procps vim nano less \
      # Version control
      git \
      # Build tools (for native Node.js modules)
      build-essential python3 python3-pip \
      # Additional utilities
      zip unzip jq htop tree && \
    rm -rf /var/lib/apt/lists/*

# Install Node.js 20 LTS
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list && \
    apt-get update && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

# Install Chrome dependencies for web scraping
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      # Chrome dependencies
      libnss3 libnspr4 libatk1.0-0 libatk-bridge2.0-0 \
      libcups2 libdrm2 libxkbcommon0 libxcomposite1 \
      libxdamage1 libxfixes3 libxrandr2 libgbm1 \
      libpango-1.0-0 libcairo2 libasound2 libatspi2.0-0 \
      libxshmfence1 libglib2.0-0 libnss3 libx11-6 \
      libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 \
      libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 \
      libxrender1 libxss1 libxtst6 \
      # Chinese fonts support
      fonts-wqy-zenhei fonts-wqy-microhei \
      fonts-noto-cjk fonts-noto-cjk-extra \
      fonts-liberation fonts-ipafont-gothic fonts-thai-tlwg fonts-kacst \
      # Additional tools
      xdg-utils && \
    rm -rf /var/lib/apt/lists/*

# Download and install Google Chrome
RUN wget -q -O /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    apt-get update && \
    apt-get install -y /tmp/google-chrome-stable_current_amd64.deb && \
    rm /tmp/google-chrome-stable_current_amd64.deb && \
    rm -rf /var/lib/apt/lists/*

# Set Puppeteer environment variables
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/google-chrome-stable

# Install pnpm globally
RUN npm install -g pnpm@10.15.1

# Install Claude Code CLI globally
RUN npm install -g @anthropic-ai/claude-code

# Create symlink for claude-code command (SDK expects this name)
RUN ln -s /usr/bin/claude /usr/bin/claude-code

# Verify installations
RUN node --version && \
    npm --version && \
    pnpm --version && \
    git --version && \
    python3 --version && \
    google-chrome --version

# Create node user (UID=1000 for host compatibility)
RUN useradd -u 1000 -U -m -s /bin/bash node && \
    echo "node ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Default work directory
WORKDIR /app

# Default user (can be overridden)
USER node

CMD ["/bin/bash"]

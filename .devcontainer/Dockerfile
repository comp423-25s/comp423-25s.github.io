FROM python:latest

# Set the working directory
WORKDIR /workspace

# Install dev tools
RUN apt-get update && apt-get install -y \
    vim \
    git \
    curl \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Create and use the vscode user
RUN useradd -m vscode && \
    chsh -s /bin/bash vscode && \
    chown -R vscode:vscode /workspace

# Switch to the vscode user
USER vscode

# Set PATH to include user-installed pip binaries
ENV PATH="/home/vscode/.local/bin:$PATH"

# Set the default command
CMD ["sleep", "infinity"]
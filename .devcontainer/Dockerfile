# Base debian build (latest).
FROM mcr.microsoft.com/vscode/devcontainers/base:debian

# Update packages.
RUN apt-get update

# Set the default shell to zsh
ENV SHELL=/usr/bin/zsh

# Running everything under zsh
SHELL ["/usr/bin/zsh", "-c"]

# Dropping privileges
USER vscode

# Install rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y && source $HOME/.cargo/env

# Install uv and add to PATH
# See https://docs.astral.sh/uv/guides/integration/docker/
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
ENV PATH="/home/vscode/.local/bin:$PATH"

# Add uv to shell configuration
RUN echo 'export PATH="/home/vscode/.cargo/bin:$PATH"' >> ~/.zshrc

# Install tools using uv
RUN uv tool install solc-select
RUN uv tool install slither-analyzer
RUN uv tool install crytic-compile

# Foundry framework
RUN curl -L https://foundry.paradigm.xyz | zsh
RUN foundryup

# Aderyn
RUN curl -L https://raw.githubusercontent.com/Cyfrin/up/main/install | zsh
RUN cyfrinup

# Clean up
RUN sudo apt-get autoremove -y && sudo apt-get clean -y

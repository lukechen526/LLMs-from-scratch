FROM mcr.microsoft.com/devcontainers/python:1-3.11-bookworm

# Common system packages used by ML/Python workflows.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        git \
        curl \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

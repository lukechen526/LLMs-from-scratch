## Base image with PyTorch 2.8, CUDA 12.9
FROM pytorch/pytorch:2.8.0-cuda12.9-cudnn9-runtime

## System dependencies
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    rsync git curl ca-certificates git-lfs openssh-client && \
    rm -rf /var/lib/apt/lists/*

## Create a dedicated virtual environment (do not pollute system site-packages)
ENV VENV_PATH=/opt/venv
RUN python -m venv "$VENV_PATH" \
    && "$VENV_PATH/bin/pip" install --no-cache-dir --upgrade pip
ENV VIRTUAL_ENV=$VENV_PATH
ENV PATH="$VENV_PATH/bin:$PATH"

## Install uv (used for fast, reproducible installs)
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin:$PATH"

## Copy dependency manifests early for better layer caching
COPY requirements.txt ./
# (Optional) If you prefer pyproject + lock, uncomment and copy them instead:
# COPY pyproject.toml uv.lock ./

## Install Python dependencies into the virtual environment
# Using uv targeting the venv's python for speed & resolution
RUN uv pip install --python "$VENV_PATH/bin/python" --no-cache -r requirements.txt
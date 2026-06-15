FROM python:3.14-slim-bookworm

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        aspell \
        aspell-en && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pyspelling

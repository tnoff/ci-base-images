ARG PYTHON_VERSION=3.14
FROM python:${PYTHON_VERSION}-slim-bookworm

# Kitchen-sink apt set: union of system deps used by the Python fleet at CI
# time. Adding a new dep here makes it available to every Python repo that
# sets TOX_BASE_IMAGE — slightly larger pull for lean consumers vs. zero
# per-consumer image variants.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        ffmpeg \
        postgresql-client && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

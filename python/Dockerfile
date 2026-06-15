ARG PYTHON_VERSION=3.14
FROM python:${PYTHON_VERSION}-slim-bookworm

# Kitchen-sink apt set: union of system deps used by the Python fleet at CI
# time. Adding a new dep here makes it available to every Python repo that
# sets TOX_BASE_IMAGE — slightly larger pull for lean consumers vs. zero
# per-consumer image variants.
#
# postgresql (server meta-package) — not postgresql-client — because
# discord-bot's tests use pytest-postgresql, which calls Debian's
# pg_ctlcluster to start a local cluster (see start_postgres_for_ci.sh).
# pg_ctlcluster lives in the server package; the client-only package would
# silently skip the cluster start and break every test that touches the DB.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        git \
        ffmpeg \
        postgresql && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

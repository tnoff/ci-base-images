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
        postgresql \
        zstd && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# diff-cover and tox are used by github-workflows' tox-pipeline template,
# not by project tests. Baking them here lets the tox-pipeline jobs skip
# their inline `pip install diff-cover` / `pip install tox` — the child
# tox job and the diff-cover child job rely on TOX_BASE_IMAGE; the parent
# `.tox-generate` job relies on TOX_GENERATE_IMAGE pointing at this image.
#
# awscli is the S3 client the tox-pipeline template uses to push/pull the
# .tox/ venv cache to the gitlab_tox_cache OCI bucket (paired with the apt
# `zstd` above for the cache tarballs). awscli auto-selects path-style
# addressing for the underscore bucket name and reads the reused
# buildkit-cache AWS_* creds already injected into job pods. See docs
# projects/ci-perf.md scope #9.
RUN pip install --no-cache-dir diff-cover tox awscli

FROM python:3.14-slim-bookworm

# CI image for the github-workflows `pre-commit` job — pre-commit drives git
# (staged-file discovery, from/to-ref ranges) so git is required alongside
# the pip package. Replaces the per-job `apt-get install git` + `pip install
# pre-commit` preamble.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pre-commit

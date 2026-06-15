FROM python:3.14-slim-bookworm

ARG KUBECONFORM_VERSION=0.6.7
ARG KUBE_LINTER_VERSION=0.7.2
ARG CONFTEST_VERSION=0.67.0

# Binaries are arm64-only — this image is built exclusively for linux/arm64
# (the OKE self-hosted runner pool). Update ARG defaults when bumping tool
# versions; Renovate is not yet wired for these ARGs.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        curl \
        ca-certificates && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

RUN curl -sSfL "https://github.com/yannh/kubeconform/releases/download/v${KUBECONFORM_VERSION}/kubeconform-linux-arm64.tar.gz" | tar -xz -C /usr/local/bin kubeconform && \
    curl -sSfL "https://github.com/stackrox/kube-linter/releases/download/v${KUBE_LINTER_VERSION}/kube-linter-linux_arm64.tar.gz" | tar -xz -C /usr/local/bin kube-linter && \
    curl -sSfL "https://github.com/open-policy-agent/conftest/releases/download/v${CONFTEST_VERSION}/conftest_${CONFTEST_VERSION}_Linux_arm64.tar.gz" | tar -xz -C /usr/local/bin conftest

RUN pip install --no-cache-dir pre-commit

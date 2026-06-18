FROM alpine:3

ARG TRUFFLEHOG_VERSION=3.95.6

# CI image for the .trufflehog-image template. Bakes a pinned trufflehog
# binary so the template no longer `curl | sh`-installs it from
# raw.githubusercontent.com on every scan (removes that supply-chain dep).
# The binary is arm64 — this image is built exclusively for linux/arm64 (the
# OKE self-hosted runner pool). Bump the ARG to update; Renovate is not yet
# wired for it. ca-certificates is needed for `--only-verified` detector
# calls; the trufflehog-image scan reads a docker-save tarball so no git/
# docker daemon is required.
RUN apk add --no-cache ca-certificates curl && \
    curl -sSfL "https://github.com/trufflesecurity/trufflehog/releases/download/v${TRUFFLEHOG_VERSION}/trufflehog_${TRUFFLEHOG_VERSION}_linux_arm64.tar.gz" \
      | tar -xz -C /usr/local/bin trufflehog && \
    trufflehog --version

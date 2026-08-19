FROM alpine:3

# renovate: datasource=github-releases depName=trufflesecurity/trufflehog
ARG TRUFFLEHOG_VERSION=3.97.0

# CI image for the .trufflehog-image template. Bakes a pinned trufflehog
# binary so the template no longer `curl | sh`-installs it from
# raw.githubusercontent.com on every scan (removes that supply-chain dep).
# The binary is arm64 — this image is built exclusively for linux/arm64 (the
# OKE self-hosted runner pool). The version ARG is Renovate-managed via the
# `# renovate:` annotation above (github-releases datasource in renovate.json).
# ca-certificates is needed for `--only-verified` detector
# calls; the trufflehog-image scan reads a docker-save tarball so no git/
# docker daemon is required.
#
# aws-cli is the S3 client the .trufflehog-image template uses to pull the
# docker-save tarball from OCI Object Storage instead of a GitLab job
# artifact — the image tarballs are the whole of this namespace's artifact
# storage and the handoff bucket is lifecycle-swept daily. Pre-baked rather
# than `apk add`-ed per scan for the same reason the trufflehog binary is:
# a per-job network install is a supply-chain and flake surface.
# Alpine's aws-cli package is v2 (2.34.x), unlike the pip-installed v1 in
# the python image that tox-pipeline.yml uses — so the v1 region quirk
# (AWS_DEFAULT_REGION over AWS_REGION) does not bite here, but v2's default
# integrity checksums do: OCI's S3-compatible endpoint rejects them, so the
# template exports AWS_REQUEST/RESPONSE_CHECKSUM_CALCULATION=when_required
# (the same remedy as the buildkit v0.31 aws-chunked 501) plus
# AWS_S3_ADDRESSING_STYLE=path for the underscore bucket name.
RUN apk add --no-cache ca-certificates curl aws-cli && \
    curl -sSfL "https://github.com/trufflesecurity/trufflehog/releases/download/v${TRUFFLEHOG_VERSION}/trufflehog_${TRUFFLEHOG_VERSION}_linux_arm64.tar.gz" \
      | tar -xz -C /usr/local/bin trufflehog && \
    trufflehog --version && \
    aws --version

# Pinned deliberately — see the renovate.json packageRules block that disables
# bumps for this image. buildkit v0.31 sends aws-chunked content encoding on
# the S3 cache export, which OCI Object Storage rejects with a 501, breaking
# every build that imports/exports the layer cache. Do not bump without
# re-testing the S3 cache path end to end.
FROM moby/buildkit:v0.30.0

# CI image for the .buildkit-build-check template. Upstream moby/buildkit
# ships buildkitd + buildctl and nothing else, so the build job has no way to
# put its docker-save tarball anywhere except a GitLab job artifact. Those
# tarballs were the entire namespace artifact footprint (13.9 GB of a 14.0 GB
# rollup; ~1.27 GB per discord-bot MR pipeline across five images) and
# .trufflehog-image is their only consumer, so the handoff moves to the
# gitlab_image_handoff bucket and this image carries the client that writes it.
#
# Pre-baked rather than apk add-ed per job for the same reason the trufflehog
# binary is: a per-job network install is a supply-chain and flake surface, and
# build jobs are the highest-volume jobs in the fleet.
#
# Built for linux/arm64 only (the OKE self-hosted runner pool). aws-cli here is
# alpine's v2 package; the OCI S3-compat flags the template exports
# (path addressing, when_required checksums) are the v2-relevant ones.
RUN apk add --no-cache aws-cli && \
    buildctl --version && \
    aws --version

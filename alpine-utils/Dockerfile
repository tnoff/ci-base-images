FROM alpine:3

# Lightweight CI utility image for the alpine-based shell jobs in
# github-workflows (.bump-version, .tag, .release, .trigger-bump) and
# docker-apps' bump-*-pin jobs, which all `apk add` the same handful of
# tools inline at job start. Pre-baking them removes that per-job install.
# git + curl + jq cover the GitLab API / version-bump scripts; bash because
# several of those scripts use bashisms and alpine's default shell is ash.
RUN apk add --no-cache \
        git \
        curl \
        jq \
        bash

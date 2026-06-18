# Tracks alpine/terragrunt:latest (same floating tag the .terragrunt-base job
# used inline before this image existed) — the terragrunt version advances on
# each ci-base-images rebuild from main. jq + curl are needed by the plan
# jobs' after_script (ci/post-plan-comment.sh); the base image ships neither.
FROM alpine/terragrunt:latest

RUN apk add --no-cache jq curl

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

# rendercv + PyYAML back the personal-website `generate-resume` pre-commit hook,
# which regenerates resume.html + the CV PDF from Tyler_North_CV.yaml. Baking
# them here lets that job drop its per-run `pip install rendercv[full] pyyaml`
# (rendercv depends on ruamel-yaml, but generate.py imports the pyyaml module).
RUN pip install --no-cache-dir "rendercv[full]" pyyaml

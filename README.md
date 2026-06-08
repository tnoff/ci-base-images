# ci-base-images

Python CI runtime base images, one per Python minor. Pre-baked with
`git` + the system deps used by the Python fleet's CI matrix so
downstream tox jobs can skip the per-MR apt-install preamble.

Full layout (Dockerfile, `tox.ini`, `.gitlab-ci.yml`, `renovate.json`)
lands in a follow-up MR.

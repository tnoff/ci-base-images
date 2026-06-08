# ci-base-images

Python CI runtime base images — one image per supported Python minor,
pre-baked with `git` + the OS deps the Python fleet's CI matrix needs
(currently `ffmpeg`, `postgresql-client`). Pushed to OCIR so the tox
matrix in downstream repos skips the apt-install preamble entirely.

## What this publishes

For each Python minor listed in [`tox.ini`](./tox.ini), the build pushes
two tags to the OCIR repo `ci-base-images`:

- **`:3.X`** — mutable. Overwritten on every rebuild from `main`. Use
  this for everyday consumer pinning; it auto-picks up apt/Python
  security rebuilds.
- **`:3.X-<short-sha>`** — immutable. The commit SHA of the build that
  produced it. Use if you want strict bit-for-bit reproducibility for
  a given consumer pipeline run.

## Consumer usage

Set `TOX_BASE_IMAGE` on a `.tox-generate` job in a downstream repo. Use
the group-level `CI_BASE_IMAGE_PATH` CI variable (provisioned by
`terraform/infra/gitlab.tf`) for the registry prefix, and let
`${PYTHON_VERSION}` get expanded per matrix entry by the generated
child pipeline:

```yaml
tox-generate:
  extends: .tox-generate
  variables:
    TOX_BASE_IMAGE: '${CI_BASE_IMAGE_PATH}:${PYTHON_VERSION}'
```

When `TOX_BASE_IMAGE` is set, the generator skips `apt-get install` and
only runs `pip install tox` before the tox command — `TOX_EXTRA_APT` is
ignored.

## Adding a system dep

Edit [`python/Dockerfile`](./python/Dockerfile) and add the package to
the `apt-get install` line. Open an MR; on merge, the next build
overwrites all `:3.X` tags with rebuilt images that include the new
package. Consumers don't need to change anything to pick it up.

The apt list is intentionally a single kitchen-sink set rather than
per-flavor images. The tradeoff is that lean consumers pay a few extra
MB of image pull for deps they don't use; this is much cheaper than
per-flavor tag-management complexity at this fleet size.

## Adding a Python minor

Edit BOTH [`tox.ini`](./tox.ini) (add `py3X` to `env_list`) and
[`.gitlab-ci.yml`](./.gitlab-ci.yml) (add `"3.X"` to the `build` job's
`parallel: matrix:`). The `matrix-sync-check` validate-stage job fails
the pipeline if they drift, so a single-file change won't reach `main`.
Renovate doesn't bump this — Python minors ship once a year and the
decision to start publishing one is intentional.

## Why `tox.ini`?

`tox.ini`'s `env_list` is the documented "what Python minors do we
publish" declaration. It's the same shape consumer repos use, so the
convention is uniform — and the `.tox-generate` template's
`py(\d)(\d+)` regex is the literal sync-check used by
`matrix-sync-check` here.

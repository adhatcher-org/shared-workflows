# shared-workflows

Reusable GitHub Actions workflows for adhatcher-org applications.

Callers should pin to a tagged commit SHA with the tag in a trailing comment:

```yaml
jobs:
  ci:
    uses: adhatcher-org/shared-workflows/.github/workflows/app-ci.yml@<sha> # v1.0.2
```

## `app-ci.yml`

Single `test-and-build` job that installs the toolchain, then runs each enabled
gate as a `make` target in the caller's repository. Every gate is opt-in/out via
a boolean input, so the caller's `Makefile` only needs targets for the gates it
enables.

| Input | Default | Notes |
| --- | --- | --- |
| `python-version` | *required* | |
| `dependency-manager` | *required* | `uv`, `poetry`, or `pip` |
| `project-directory` | `.` | Where the Python project lives |
| `uv-version` | `0.11.7` | |
| `poetry-version` | `2.3.2` | |
| `use-node` | `false` | Installs Node and runs `npm ci` |
| `node-version` | `22` | |
| `node-directory` | `.` | Must contain `package-lock.json` |
| `run-lint` | `false` | `make lint` |
| `run-typecheck` | `false` | `make typecheck` |
| `run-tests` | `true` | `make test` |
| `run-coverage` | `true` | `make coverage`, uploads `**/coverage.xml` |
| `run-security` | `true` | `make security` |
| `run-dependency-check` | `false` | `make dependency-check` |
| `run-pr-check` | `false` | `make pr-check` |
| `run-docker-build` | `true` | `docker build` |
| `docker-context` | `.` | |
| `dockerfile` | `Dockerfile` | |

## `codeql.yml`

CodeQL analysis. Takes a `languages` input as a JSON array string, e.g.
`'["python", "javascript-typescript"]'`.

## `docker-publish.yml`

Builds and pushes a multi-arch image to GHCR, versioned `YYYY.MM.DD-N` where `N`
increments per release that day. Intended to be called from a `workflow_run`
trigger gated on CI success on the default branch.

## Versioning

Tags are cut on `main`. Callers pin the tag's commit SHA rather than the mutable
tag ref.

| Tag | Notes |
| --- | --- |
| `v1.0.2` | Consolidated onto `main`; adds `run-dependency-check` and `run-pr-check` to `app-ci.yml`; hardened `docker-publish.yml` (SHA-pinned actions, per-repo concurrency group, user/org GHCR path detection) |
| `v1.0.1` | `app-ci.yml`, `codeql.yml`, `docker-publish.yml` (pre-consolidation, on the `codex/shared-actions` branch) |
| `v1.0.0` | Initial reusable application workflows |

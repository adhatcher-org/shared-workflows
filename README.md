# Shared workflows

Reusable GitHub Actions workflows for applications owned by `adhatcher-org`.

## Workflows

- `app-ci.yml` installs Python and optional Node dependencies, runs standardized
  Make targets, uploads coverage, and verifies the Docker image builds.
- `codeql.yml` runs CodeQL for a caller-provided language matrix.
- `docker-publish.yml` publishes multi-platform images to GHCR after CI succeeds.

Consumer workflows pin reusable workflows to an exact commit. Dependabot keeps
those references current through reviewable pull requests.
Reusable GitHub Actions workflows for adhatcher-org applications

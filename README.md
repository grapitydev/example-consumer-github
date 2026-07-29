# example-consumer-github

Live example of a Grapity consumer repository on GitHub. It materializes the
public [`grapity-registry`](https://registry.grapity.dev) spec into this repo
and demonstrates automated spec-drift detection on pull requests.

## What it demonstrates

- `grapity.yaml` + `grapity-lock.json`: the consumer-side pinning artifacts
  produced by [`grapity materialize`](https://grapity.dev/docs/cli-reference/materialize)
- `grapity/specs/grapity-registry.yaml`: the materialized spec, the input for
  your own code generators
- `.github/workflows/materialize-check.yml`: runs `grapity materialize --check`
  on every pull request and reports drift as a sticky PR comment (outdated
  specs with materialized vs latest versions, or an all-clear note). Stale
  specs also appear as inline `::warning::` annotations in the checks UI.

The consumed spec is public, so the workflow uses anonymous reads
(`auth: mode: none`) and needs no registry credentials, only a
`GRAPITY_REGISTRY_URL` repository variable. For registries that require
authentication (Keycloak client credentials), see the authenticated variant in
[`grapitydev/grapity/examples/materialize-check`](https://github.com/grapitydev/grapity/tree/main/examples/materialize-check).

## Try it yourself

1. Copy `.github/workflows/materialize-check.yml` into your own consumer repo.
2. Set the `GRAPITY_REGISTRY_URL` Actions variable to your registry URL.
3. Open a PR; the check reports spec freshness as a PR comment.

Since `grapity-registry` is republished on every Grapity release, this repo's
lockfile naturally drifts behind over time, which is exactly what the workflow
is designed to surface.

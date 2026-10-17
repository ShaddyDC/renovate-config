# renovate-config

The Renovate preset every ShaddyDC project extends:

```json
{ "extends": ["local>ShaddyDC/renovate-config"] }
```

Repo-specific rules go in that repo's own `renovate.json` after the `extends`.

What it does:

- **Automerges what CI proves works**: minor, patch, digest and pin updates, lock file
  maintenance, and GitHub Actions majors, each once the pull request's checks pass. Renovate
  never automerges a PR without passing checks, so a repo without PR CI just gets PRs.
- **Other majors wait for a review.**
- **7-day cooldown** (`minimumReleaseAge`) against compromised releases, which are usually
  caught and yanked within days. Renovate opens no PR for a version younger than that.
  Security updates skip it. Where it does not reach:
  - Docker images outside Docker Hub (ghcr.io, quay.io, registry.k8s.io): Renovate gets no
    push times from them, so they update without waiting.
  - Lock file maintenance resolves through the package manager. npm gets `--before` from
    Renovate. pnpm and uv repos set their own cooldown (`minimumReleaseAge: 10080` in
    `pnpm-workspace.yaml`, `exclude-newer = "7 days"` under `[tool.uv]`). Cargo has none on
    stable before Rust 1.100, so Cargo lock maintenance opens PRs but does not automerge.
- **Weekly**: Monday 00:00-06:00 UTC, all non-major updates in one PR, so private repos spend
  few Actions minutes on it. Security updates do not wait for the schedule.
- **Lock files in lockstep**: every repo refreshes its lock files (flake.lock included) in the
  same window, so projects land on the same nixpkgs revision and their Nix-built images share
  layers.

Repositories whose updates must stay manual keep their own config instead of extending this.

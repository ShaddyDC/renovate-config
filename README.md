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
- **Weekly**: Monday 00:00-06:00 UTC, all non-major updates in one PR, so private repos spend
  few Actions minutes on it. Security updates do not wait for the schedule.
- **Lock files in lockstep**: every repo refreshes its lock files (flake.lock included) in the
  same window, so the Nix-built CI runner images of riader and rerep land on the same nixpkgs
  revision and share their layers on the cluster's nodes.

ShaddyDC/cluster does not extend this: its merges and deploys stay manual.

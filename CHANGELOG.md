# Changelog

- 2026-11-09
  - The Tool drift workflow checks language runtimes: `PYTHON_VERSION` and `NODE_VERSION` pins and the Go version in `go.mod` are compared with their line's latest patch release on endoflife.date, and an issue opens when a newer patch exists or the line is within 183 days of, or past, its end of life. `PHP_VERSION`, which names the runner image's PHP, is skipped instead of warned about. A failed lookup now fails the job after the remaining pins are checked, instead of stopping at the first. Found internally from backlog item 382.
- 2026-10-24
  - The continuous integration (CI) secret scan downloads gitleaks 8.30.1, up from 8.21.2, verified by its checksum, and the download accepts only HTTPS with Transport Layer Security (TLS) 1.2 or later (`curl --proto "=https" --tlsv1.2`), so a redirect cannot drop it to plain HTTP. Found internally from backlog item 381.
- 2026-10-23
  - Added continuous integration (CI): yamllint, actionlint, the pre-commit hooks, and a gitleaks scan of the checked-out history, with Dependabot for the actions, the Python tools, and the hooks.
  - Added the reusable Tool drift workflow, which opens an issue when a release binary pinned in a workflow falls behind its latest release.
  - Added a gitleaks pre-push hook, so a commit holding a secret is refused before it is pushed.

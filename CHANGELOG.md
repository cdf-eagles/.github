# Changelog

- 2026-10-24
  - The continuous integration (CI) secret scan downloads gitleaks 8.30.1, up from 8.21.2, verified by its checksum, and the download accepts only HTTPS with Transport Layer Security (TLS) 1.2 or later (`curl --proto "=https" --tlsv1.2`), so a redirect cannot drop it to plain HTTP. Found internally from backlog item 381.
- 2026-10-23
  - Added continuous integration (CI): yamllint, actionlint, the pre-commit hooks, and a gitleaks scan of the checked-out history, with Dependabot for the actions, the Python tools, and the hooks.
  - Added the reusable Tool drift workflow, which opens an issue when a release binary pinned in a workflow falls behind its latest release.
  - Added a gitleaks pre-push hook, so a commit holding a secret is refused before it is pushed.

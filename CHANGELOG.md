# Changelog

- 2026-11-23
  - Continuous integration (CI) retries each pinned tool download (`--retry 8 --retry-delay 30 --retry-max-time 300 --retry-all-errors`) for up to about five minutes, so a passing error from GitHub's release download service, such as the 504 replies of 2026-11-08, no longer fails the job before its checks run; the SHA-256 check after each download is unchanged. Found internally from backlog item 516.
- 2026-11-22
  - Continuous integration (CI) installs Python 3.13.18 with `actions/setup-python` (`PYTHON_VERSION` in the workflow's `env:` block) in every job that runs Python, instead of using the runner image's own `python3`, whose version changed with the image. The weekly Tool drift job checks the pin against its line's latest patch release. Found internally from backlog item 536.
- 2026-11-18
  - A Snyk job in continuous integration (CI) tests the Python tools pinned in `ci/requirements.txt`, which Snyk's repository import skips, and fails on any high or critical issue, a scan error, or a missing `SNYK_TOKEN` secret. The Snyk CLI 1.1308.4 is downloaded from its GitHub release and verified by its checksum; found internally from backlog item 572.
- 2026-11-13
  - The Tool drift workflow compares a `SNYK_VERSION` pin with the latest release of snyk/cli and opens an issue when it falls behind; it only warned that the pin had no known upstream. Found internally from backlog item 546.
- 2026-11-09
  - The Tool drift workflow checks language runtimes: `PYTHON_VERSION` and `NODE_VERSION` pins and the Go version in `go.mod` are compared with their line's latest patch release on endoflife.date, and an issue opens when a newer patch exists or the line is within 183 days of, or past, its end of life. `PHP_VERSION`, which names the runner image's PHP, is skipped instead of warned about. A failed lookup now fails the job after the remaining pins are checked, instead of stopping at the first. Found internally from backlog item 382.
- 2026-10-24
  - The continuous integration (CI) secret scan downloads gitleaks 8.30.1, up from 8.21.2, verified by its checksum, and the download accepts only HTTPS with Transport Layer Security (TLS) 1.2 or later (`curl --proto "=https" --tlsv1.2`), so a redirect cannot drop it to plain HTTP. Found internally from backlog item 381.
- 2026-10-23
  - Added continuous integration (CI): yamllint, actionlint, the pre-commit hooks, and a gitleaks scan of the checked-out history, with Dependabot for the actions, the Python tools, and the hooks.
  - Added the reusable Tool drift workflow, which opens an issue when a release binary pinned in a workflow falls behind its latest release.
  - Added a gitleaks pre-push hook, so a commit holding a secret is refused before it is pushed.

# Changelog

- 2026-10-23
  - Added continuous integration (CI): yamllint, actionlint, the pre-commit hooks, and a gitleaks scan of the checked-out history, with Dependabot for the actions, the Python tools, and the hooks.
  - Added the reusable Tool drift workflow, which opens an issue when a release binary pinned in a workflow falls behind its latest release.
  - Added a gitleaks pre-push hook, so a commit holding a secret is refused before it is pushed.

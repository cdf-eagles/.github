# .github

Default community health files for the account's public repositories. `SECURITY.md` applies to every repository that has no security policy of its own.

## Tool drift

`.github/workflows/tool-drift.yaml` is a reusable workflow for the account's repositories. A repository's weekly CI run calls it; it reads every `<TOOL>_VERSION` pin in that repository's workflows (gitleaks, actionlint, terraform-docs, nfpm, OpenTofu as `TOFU_VERSION`, tflint, and the Snyk CLI as `SNYK_VERSION`), compares it with the tool's latest release, and opens or updates one issue per tool that is behind. Tools whose version lives in a manifest that Dependabot reads are not pinned this way and are not checked here.

Language runtimes are checked by their supported line rather than a GitHub release: `PYTHON_VERSION` and `NODE_VERSION` pins, pinned to a full patch release, and the Go version in `go.mod` (its `toolchain` line when it has one, otherwise its `go` line). For each, the workflow reads the line's entry on [endoflife.date](https://endoflife.date) and opens an issue when a newer patch release of the line exists, when the line is within `EOL_WARN_DAYS` (183) days of its end of life, or when the line has reached it. A `PHP_VERSION` pin names the runner image's PHP, which the repository's own CI checks, so it is skipped. A lookup that fails is an error and fails the job after every other pin has been checked.

A repository calls it from a job that runs only on the schedule:

```yaml
  tool-drift:
    name: Tool drift
    if: github.event_name == 'schedule'
    permissions:
      contents: read
      issues: write
    uses: cdf-eagles/.github/.github/workflows/tool-drift.yaml@<commit SHA>  # <tag>
```

## Checks

`pre-commit run --all-files` runs the same hooks as CI. `pre-commit install` installs them at commit and push time; the push hook runs gitleaks over the commits being pushed.

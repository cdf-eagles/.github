# .github

Default community health files for the account's public repositories. `SECURITY.md` applies to every repository that has no security policy of its own.

## Tool drift

`.github/workflows/tool-drift.yaml` is a reusable workflow for the account's repositories. A repository's weekly CI run calls it; it reads every `<TOOL>_VERSION` pin in that repository's workflows (gitleaks, actionlint, terraform-docs, nfpm, OpenTofu as `TOFU_VERSION`, and tflint), compares it with the tool's latest release, and opens or updates one issue per tool that is behind. Tools whose version lives in a manifest that Dependabot reads are not pinned this way and are not checked here.

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

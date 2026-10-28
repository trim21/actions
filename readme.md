## Pre-Commit

```yaml
name: lint

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: "3.10"
      - run: pip install pre-commit
      - uses: trim21/actions/pre-commit@master
```

## Prek

```yaml
name: lint

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout
      - uses: trim21/actions/prek@master
```

## govulncheck

Runs `govulncheck` and keeps one tracking issue in sync with the result: the issue is
created (or its body refreshed) when vulnerabilities are found, and closed once a scan
comes back clean. Requires `permissions: issues: write`, and `go` must be installed
already (e.g. through `trim21/actions/setup-go`).

`report-issues` defaults to `false`. Use the expression below so that pull requests only
report the scan result without touching issues.

```yaml
name: security

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master
  schedule:
    - cron: "15 3 * * 4"

permissions:
  contents: read
  issues: write

jobs:
  vulncheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout
      - uses: trim21/actions/setup-go@master
        with:
          cache-namespace: vulncheck
      - uses: trim21/actions/govulncheck@master
        with:
          go-tags: release
          report-issues: ${{ github.event_name != 'pull_request' }}
```

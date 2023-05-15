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

# .pypirc

```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
repository = https://upload.pypi.org/legacy/

[testpypi]
repository = https://test.pypi.org/legacy/
```




# pyproject.toml
```ini
[tool.flake8]
max-line-length = 120
select = "F,E,W,B,B901,B902,B903"
exclude = [
    ".eggs",
    ".git",
    ".tox",
    "nssm",
    "obj",
    "out",
    "packages",
    "pywin32",
    "tests",
    "swagger_client"
]
ignore = [
    "E722",
    "B001",
    "W503",
    "E203"
]
```

github workflow publish
```yaml
name: Publish PyPI

on:
  push:
    tags:
      - 'v*'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.7'
      - name: Publish packages
        env:
          FLIT_USERNAME: __token__
          FLIT_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
        run: |
          pip install flit
          flit publish --setup-py
```

github `dependabot.yml`
```yaml
# To get started with Dependabot version updates, you'll need to specify which
# package ecosystems to update and where the package manifests are located.
# Please see the documentation for all configuration options:
# https://docs.github.com/github/administering-a-repository/configuration-options-for-dependency-updates

version: 2
updates:
  - package-ecosystem: "github-actions" # See documentation for possible values
    directory: "/" # Location of package manifests
    schedule:
      interval: "weekly"
```

# Workflow templates

## Poetry tests

To use in a given workflow:
```yaml
jobs:
  job-name:
    uses: alercebroker/workflow-templates/.github/workflows/poetry-tests.yml@main
    with:
      python-version: '3.9'
      poetry-version: '1.3.1'
      sources-folder: 'package'
      unittest-folder: 'tests/unittests'
      unittest-flags: 'unittests'
      integration-folder: 'tests/integration'
      integration-flags: 'integration'
      base-folder: '.'
```

All inputs are optional and all expect strings:
* `python-version` (default is `'3.10'`): Python version to use
* `poetry-version` (default is `'1.3.1'`): Poetry version to use
* `base-folder` (default is `'.'`): Root folder for the package. For standard repositories it doesn't need to be provided, but for monorepos this usually refers to whatever the root folder leading to the `poetry.lock`/`pyproject.toml` file of interes is 
* `sources-folder` (default is `'src'`): Folder with source files (with respect to `base-folder`)
* `unittest-folder` (default is `'tests'`): Folder with files for unittest (with respect to `base-folder`)
* `unittest-flags` (default is `'unittest'`): Flags used when uploading unittest coverage to Codecov
* `integration-folder` (default is `''`): Folder with files for integration tests (with respect to `base-folder`). If using the default, assumes will skip integration tests
* `integration-flags` (default is `''`): Flags used when uploading integration test coverage to Codecov. If using the default, it will skip uploading the coverage report

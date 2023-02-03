# Workflow templates

## Lint

To use in a given workflow:
```yaml
jobs:
  job-name:
    uses: alercebroker/workflow-templates/.github/workflows/lint.yml@main
    with:
      sources-folder: 'package'
      base-folder: '.'
```

All inputs are optional and all expect strings:
* `base-folder` (default is `'.'`): Root folder for the package. For standard repositories it doesn't need to be provided, but for monorepos this usually refers to whatever the root folder leading to the `poetry.lock`/`pyproject.toml` file of interes is 
* `sources-folder` (default is `'src'`): Folder with source files (with respect to `base-folder`)

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
* `base-folder` (default is `'.'`): Root folder for the package. For standard repositories it doesn't need to be provided, but for monorepos this usually refers to whatever the root folder leading to where the `poetry.lock` or `pyproject.toml` file of interes is 
* `sources-folder` (default is `'src'`): Folder with source files (with respect to `base-folder`)
* `test-folder` (default is `'tests'`): Folder with files for test (with respect to `base-folder`)
* `codecov-flags` (default is `'unittest'`): Flags used when uploading test coverage to Codecov. If passing an empty string, it will not upload anything


## PIP tests

To use in a given workflow:
```yaml
jobs:
  job-name:
    uses: alercebroker/workflow-templates/.github/workflows/pip-tests.yml@main
    with:
      python-version: '3.9'
      sources-folder: 'package'
      test-folder: 'tests/unittests'
      test-dependencies: 'pytest pytest-cov'
      codecov-flags: 'unittests'
      base-folder: '.'
```

All inputs are optional and all expect strings:
* `python-version` (default is `'3.10'`): Python version to use
* `base-folder` (default is `'.'`): Root folder for the package. For standard repositories it doesn't need to be provided, but for monorepos this usually refers to whatever the root folder leading to where the `requirements.txt` file of interes is 
* `sources-folder` (default is `'src'`): Folder with source files (with respect to `base-folder`)
* `test-folder` (default is `'tests'`): Folder with files for test (with respect to `base-folder`)
* `test-dependencies` (default is `'pytest pytest-cov'`): Additional dependencies needed for tests. It should always include pytest and pytest-cov at the very least
* `codecov-flags` (default is `'unittest'`): Flags used when uploading test coverage to Codecov. If an empty string is provided, it will skip the uploading step


# Examples

## Linting and testing in a standard repo

With PIP:

```yaml
name: Lint and test
on:
  pull_request:
    branches:
      - main

jobs:
  lint:
    uses: alercebroker/workflow-templates/.github/workflows/lint.yml@main
    with:
      sources-folder: 'package-name'
  unittest:
    uses: alercebroker/workflow-templates/.github/workflows/pip-tests.yml@main
    with:
      python-version: '3.8'
      sources-folder: 'package-name'
      test-folder: 'tests/unittest'
  integration:
    uses: alercebroker/workflow-templates/.github/workflows/pip-tests.yml@main
    with:
      python-version: '3.8'
      sources-folder: 'package-name'
      test-folder: 'tests/integration'
      test-dependencies: 'pytest pytest-docker'
      codecov-flags: ''  # Do not upload
```

With Poetry:

```yaml
name: Lint and test
on:
  pull_request:
    branches:
      - main

jobs:
  lint:
    uses: alercebroker/workflow-templates/.github/workflows/lint.yml@main
    with:
      sources-folder: 'package-name'
  unittest:
    uses: alercebroker/workflow-templates/.github/workflows/poetry-tests.yml@main
    with:
      python-version: '3.8'
      sources-folder: 'package-name'
      test-folder: 'tests/unittest'
  integration:
    uses: alercebroker/workflow-templates/.github/workflows/poetry-tests.yml@main
    with:
      python-version: '3.8'
      sources-folder: 'package-name'
      test-folder: 'tests/integration'
      codecov-flags: ''  # Do not upload
```

## Linting and testing in a monorepo

With PIP:

```yaml
name: Lint and test
on:
  pull_request:
    branches:
      - main
    paths:
      - 'libs/package-name/**'

jobs:
  lint:
    uses: alercebroker/workflow-templates/.github/workflows/lint.yml@main
    with:
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
  unittest:
    uses: alercebroker/workflow-templates/.github/workflows/pip-tests.yml@main
    with:
      python-version: '3.8'
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
      test-folder: 'tests/unittest'
  integration:
    uses: alercebroker/workflow-templates/.github/workflows/pip-tests.yml@main
    with:
      python-version: '3.8'
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
      test-folder: 'tests/integration'
      test-dependencies: 'pytest pytest-docker'
      codecov-flags: ''  # Do not upload
```

With Poetry:

```yaml
name: Lint and test
on:
  pull_request:
    branches:
      - main
    paths:
      - 'libs/package-name/**'

jobs:
  lint:
    uses: alercebroker/workflow-templates/.github/workflows/lint.yml@main
    with:
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
  unittest:
    uses: alercebroker/workflow-templates/.github/workflows/poetry-tests.yml@main
    with:
      python-version: '3.8'
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
      test-folder: 'tests/unittest'
  integration:
    uses: alercebroker/workflow-templates/.github/workflows/poetry-tests.yml@main
    with:
      python-version: '3.8'
      base-folder: 'libs/package-name'
      sources-folder: 'package-name'
      test-folder: 'tests/integration'
      codecov-flags: ''  # Do not upload
```

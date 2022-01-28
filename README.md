# Reusable Workflows

Here is a collection of (a) [reusable workflow][] that I use in several projects.

[reusable workflow]: https://docs.github.com/en/actions/using-workflows/reusing-workflows


## publish-to-pypi.yml

This workflow is used to publish python projects to PyPI.  It is assumed that:
- the project is [PEP-517][] compliant
- the project uses [setuptools_scm][setuptools-scm] for versioning

The workflow does some sanity checks before attempting to build and upload
a distribution.

### Example Usage

Here’s an example workflow that calls this one.
```yml
name: Publish to PyPI

on:
  # Run workflow when github release is made
  release:
    types: [published]

jobs:
  # Paranoia - assert tests pass before attempting upload
  tests:
    uses: ./.github/workflows/tests.yml

  publish:
    needs: [tests]
    uses: dairiki/gh-workflows/.github/workflows/publish-to-pypi.yml@v1
    secrets:
      # You could also set TEST_PYPI_API_TOKEN here if you want to attempt
      # upload to the Test PyPI.
      PYPI_API_TOKEN: ${{ secrets.PYPI_API_TOKEN }}
```

[PEP-517]: https://www.python.org/dev/peps/pep-0517/
[setuptools-scm]: https://github.com/pypa/setuptools_scm/


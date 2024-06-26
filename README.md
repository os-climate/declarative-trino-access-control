
> [!IMPORTANT]
> On June 26 2024, Linux Foundation announced the merger of its financial services umbrella, the Fintech Open Source Foundation ([FINOS](https://finos.org)), with OS-Climate, an open source community dedicated to building data technologies, modeling, and analytic tools that will drive global capital flows into climate change mitigation and resilience; OS-Climate projects are in the process of transitioning to the [FINOS governance framework](https://community.finos.org/docs/governance); read more on [finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg](https://finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg)

# osc-trino-acl-dsl
A declarative format for configuring Trino access control

To operationalize this code you need Trino adminstrator privileges

### examples:

#### Converting Trino ACL DSL to a rules.json
```sh
# install package using pipenv, pip or similar tools
$ pipenv install osc-trino-acl-dsl

# the package command `trino-dsl-to-rules` will load the given yaml or json file
# and write the resulting 'rules.json' file to standard output
# dsl-example-1.yaml is in the 'examples' directory of this repository
$ pipenv run trino-dsl-to-rules dsl-example-1.yaml > rules.json

# rules.json is trino file-based access control rules file
$ head rules.json
{
    "catalogs": [
        {
            "group": "admins",
            "allow": "all"
        },
        {
            "group": ".*",
            "catalog": "dev",
            "allow": "all"
```

#### Using pre-commit checks
For more information on pre-commit checks, see [here](https://pre-commit.com/)

Here is an example entry for `.pre-commit-config.yaml`
For more info see [here](https://github.com/os-climate/osc-trino-acl-dsl/blob/main/.pre-commit-hooks.yaml)
```yaml
repos:
  - repo: https://github.com/os-climate/osc-trino-acl-dsl
    rev: v0.3.1
    hooks:
      # a pre-commit check to verify that an ACL DSL yaml file is in sync with rules.json file
      - id: trino-acl-dsl-check
```

### building and testing

#### iterative dev/test for pre-commit checks

1. check out this repository
1. make some change to precommit checks you want to test
1. in a test repository, make an edit you expect your precommit check to operate on, then `git add` this edit (i.e. stage it for commit) but do NOT commit it, so the precommit check sees it and properly provides staged files to the argument list.
1. run `pre-commit try-repo /path/to/osc-trino-acl-dsl --verbose` (see [here](https://pre-commit.com/#pre-commit-try-repo))
1. examine the output of your precommit check to see if it did what you want

#### publish new version to pypi
- update all occurrences of `__version__` (try `git grep version`)
- `python3 setup.py clean` or `git clean -fdx`
- `python3 setup.py sdist`
- `twine check dist/*`
- `twine upload dist/*`
- push latest to repo
- create new release tag on github

upload test or release candidate:
- twine upload --repository-url https://test.pypi.org/legacy/ dist/*

### python packaging resources

- https://packaging.python.org/
- https://packaging.python.org/tutorials/packaging-projects/
- https://realpython.com/pypi-publish-python-package/


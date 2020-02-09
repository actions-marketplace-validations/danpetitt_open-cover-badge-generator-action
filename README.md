# Github Action OpenCover Badge Generator

Simple action to generate code coverage badges from a previously created dotnet [OpenCover XML](https://github.com/OpenCover/opencover) file generated by tools such as [coverlet](https://github.com/tonerdo/coverlet).

## Usage

First ensure you have a valid `opencover.xml` code coverage file generated by something like [coverlet](https://github.com/tonerdo/coverlet). For example, using the dotnet core command `dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover`.

Then, generate an access token and save as a GitHub Secret which you can then reference in your Project Workflow yaml as follows:

```yaml
on:
  pull_request:
    types: [opened]
    branches:
      - master
jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - name: OpenCover Badge Generator
        uses: danpetitt/open-cover-badge-generator-action@v1.0.9
        with:
          path-to-opencover-xml: ./test/opencover.xml
          path-to-badges: ./
          minimum-coverage: 75
          repo-token: ${{ secrets.CI_TOKEN }}
```

_(Note: Ensure you get the case of the paths correct for the agent running the job.)_

Once generated you will get two badges called `coverage-badge-line.svg` and `coverage-badge-branch.svg` saved into the path you specified which you can then add to your project readme like:

```markdown
[![Line Coverage Status](./coverage-badge-line.svg)](https://github.com/danpetitt/open-cover-badge-generator-action/)

[![Branch Coverage Status](./coverage-badge-branch.svg)](https://github.com/danpetitt/open-cover-badge-generator-action/)
```

## Inputs

### `path-to-opencover-xml` (Required)

Path to the open cover xml file

### `path-to-badges` (Optional)

**Optional:** Path where the line and branch coverage svgs would be saved; these will be saved with the names `coverage-badge-line.svg` and `coverage-badge-branch.svg`; if not specified the files will be saved into the project root. Default `"./"`.

### `minimum-coverage` (Required)

Threshold percentage at which a red badge would appear.

### `repo-token` (Required)

GitHub repo token so that the changed file can be committed, like `${{ secrets.CI_TOKEN }}`

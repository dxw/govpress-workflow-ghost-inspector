# GovPress Workflow: Ghost Inspector

A reusable workflow to run Ghost Inspector test suites. It take a Ghost Inspector test suite ID and API Key, and returns the status of that test suite.

The primary intention is that this workflow will be used in production deployment PRs, to run the test suite on staging as part of the CI pipeline, but it could in theory be used in any scenario where we want to check the status of a test suite as part of CI.

## Usage

To use this in a repo, following the instructions for [calling a reusable workflow](https://docs.github.com/en/actions/using-workflows/reusing-workflows#calling-a-reusable-workflow), passing `SUITE_ID` and `API_KEY` as secrets. The values of those secrets could be stored e.g. as secrets at individual repo level.

### Example usage

This example would result in the test suite being run on each pull request event.

In this example, the API key and suite ID are stored as repo-level secrets in the calling repo.

```yml
name: Run Ghost Inspector test suite

on: [pull_request]

jobs:

  run-test-suite:
    uses: dxw/govpress-workflow-ghost-inspector/.github/workflows/ghost-inspector.yml@v1
    secrets:
      API_KEY: ${{ secrets.GHOST_INSPECTOR_API_KEY }}
      SUITE_ID: ${{ secrets.GHOST_INSPECTOR_TEST_SUITE_ID_STAGING }}
```


## Testing locally

You can test the workflow locally using [act](https://github.com/nektos/act).

Install act:

```bash
brew install act
```

Test the action (from the root directory of this repo)

```bash
act -j ghost-inspector-run -s API_KEY=[your-ghost-inspector-api-key] -s SUITE_ID=[a-ghost-inspector-suite-id]
```

If the test suite passes, the action should exit with a success code, otherwise it will exit with a failure code (including if the test suite does not exist).

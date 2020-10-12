# GitHub Action: Process junit reports

This action processes junit XML reports on pull requests and shows the result as a PR check with summary and annotations.

![Screenshot](./screenshot.png)

## Inputs

### `github_token`

**Required**. Usually in form of `github_token: ${{ secrets.GITHUB_TOKEN }}`.

### `report_paths`

Optional. [Glob](https://github.com/actions/toolkit/tree/master/packages/glob) expression to junit report paths. The default is `**/junit-reports/TEST-*.xml`.

### `check_name`

Optional. Check name to use when creating a check run. The default is `Test Report`.

### `commit`

Optional. The commit sha to update the status. This is useful when you run it with `workflow_run`.

## Example usage

```yml
name: build
on:
  pull_request:

jobs:
  build:
    name: Build and Run Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v1
      - name: Build and Run Tests
        run: mvn test --batch-mode -Dmaven.test.failure.ignore=true
      - name: Publish Test Report
        uses: mikepenz/action-junit-report@{specific-sha1}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Tips for Gradle

As Gradle uses a different build directory than Maven by default, you might need to set the `report_paths` variable:

```yaml
    report_paths: '**/build/test-results/test/TEST-*.xml'
```

You also need to enable JUnit XML reports as shown below.

```groovy
test {
  reports {
    junitXml.enabled = true
  }
}
```

## Note

Forked from: https://github.com/ScaCap/action-surefire-report


# License

    Copyright 2020 ScaCap
    Modifications Copyright (C) 2020 Mike Penz

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

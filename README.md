# Deployment Tar Artifact Upload

[![GitHub Release](https://img.shields.io/github/release/petemcw/deploy-artifact-upload.svg?logo=github&style=for-the-badge)](https://github.com/petemcw/deploy-artifact-upload/releases/latest)
[![GitHub Marketplace](https://img.shields.io/badge/marketplace-petemcw--deploy--artifact--upload-blue?logo=github&style=for-the-badge)](https://github.com/marketplace/actions/deployment-tar-artifact-upload)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A GitHub Action to create a bundled Tar file of the workspace contents that retains filesystem permissions and excludes
defined patterns and uploads via `actions/upload-artifact`.

## ⬇️ Inputs

|        Input        |                             Description                  |Required |         Default             |
|:---------------------|:-----------------------------------------------------------|:------:|:-----------------------------|
| `name`              | The name of the bundled artifact file.                   | `false` | `VERSION`                   |
| `exclude-file`      | A file describing paths or wildcard patterns to exclude. | `true`  | `${{ github.sha }}`         |
| `if-no-files-found` | The desired behavior if no files are found.              | `false` | `${{ github.run_id }}`      |
| `retention-days`    | Duration after which artifact will expire in days.       | `false` | `${{ github.run_number }}`  |

## ⚙️ Usage

The following is a simple example of using this action during a deployment workflow.

```yaml
# File: .github/workflows/workflow.yml
name: "Production deployment"

on:
  push:
    tags:
      - "v[0-9]+.[0-9]+.[0-9]+.[0-9]+"

jobs:
  deploy-project-artifact:
    runs-on: ubuntu-latest
    steps:
      - name: "Check out repository code"
        uses: "actions/checkout@v3"

      - name: "Magento 2: build and upload artifact"
        uses: "petemcw/deploy-artifact-upload@v1"
        with:
          exclude-file: "${{ github.workspace }}/deploy/.excludes"
          if-no-files-found: "error"
          name: "magento2-artifact"
          retention-days: 1
```

## 📝 License

The scripts and documentation in this project are released under the [MIT License](LICENSE).

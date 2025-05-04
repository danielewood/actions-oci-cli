# OCI CLI Setup Action

This GitHub Action installs and configures the Oracle Cloud Infrastructure (OCI) CLI with provided credentials.

## Inputs

| Name          | Description                        | Required |
|---------------|------------------------------------|----------|
| `user`        | user OCID                          | Yes      |
| `fingerprint` | api key fingerprint                | Yes      |
| `api-key`     | api key (pem format)               | Yes      |
| `tenancy`     | tenancy OCID                       | Yes      |
| `region`      | home region (e.g., us-phoenix-1)   | Yes      |

## Example Usage

Add this action to your workflow to set up the OCI CLI and use it to interact with OCI services.

```yaml
name: Example Workflow
on:
  push:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup OCI CLI
        uses: danielewood/actions-oci-cli@v1
        with:
          user: ${{ vars.OCI_USER_ID }}
          api-key: ${{ secrets.OCI_USER_APIKEY }}
          fingerprint: ${{ vars.OCI_USER_APIKEY_FINGERPRINT }}
          tenancy: ${{ vars.OCI_TENANCY }}
          region: us-phoenix-1

      - name: List OKE Clusters
        run: |
          oci ce cluster list --compartment-id ${{ vars.OCI_TENANCY }} --query 'data[].name'
```

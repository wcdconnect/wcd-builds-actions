# WCD Builds Actions

Lightweight composite actions for integrating CI/CD pipelines with [WCD Builds](https://github.com/wcdconnect/wcd-builds) build tracking and reporting.

**This repository is auto-generated.** The source of truth for these actions lives in the main application repository at [`.github/actions/wcd-builds/`](https://github.com/wcdconnect/wcd-builds/tree/main/.github/actions/wcd-builds). Changes are synced here automatically by a GitHub Actions workflow whenever the source files change on `main`.

Do not edit files in this repository directly. Submit changes to the source repository instead.

## Actions

| Action | Purpose |
|--------|---------|
| **init** | Derive build service URL, health check, acquire Entra ID token |
| **create-build** | Register a new build with the API |
| **update-stage** | Update the build stage (e.g. Restoring, Building, Testing, Packaging, Publishing) |
| **upload-trx** | Find and upload TRX test results |
| **finalize** | Report success/failure, write dashboard link to job summary |

## Environment Variables (set by the actions)

| Variable | Set by | Used by |
|----------|--------|---------|
| `WCD_BUILDS_URL` | init | All subsequent actions |
| `WCD_BUILDS_AVAILABLE` | init | All subsequent actions (skip if `false`) |
| `WCD_BUILDS_TOKEN` | init | create-build, update-stage, upload-trx, finalize |
| `WCD_BUILD_ID` | create-build | update-stage, upload-trx, finalize |

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: wcdconnect/wcd-builds-actions/init@main
    with:
      client_id: ${{ secrets.BUILDS_CLIENT_ID }}
      client_secret: ${{ secrets.BUILDS_CLIENT_SECRET }}
      tenant_id: ${{ secrets.BUILDS_TENANT_ID }}

  - uses: wcdconnect/wcd-builds-actions/create-build@main
    with:
      app_name: "my-project"

  # ... your restore, build, test steps, with update-stage before each ...

  - if: always()
    uses: wcdconnect/wcd-builds-actions/upload-trx@main

  - if: always()
    uses: wcdconnect/wcd-builds-actions/finalize@main
    with:
      job_status: ${{ job.status }}
```

For full documentation see the [WCD Builds docs](https://github.com/wcdconnect/wcd-builds/tree/main/docs/app/using-workflow.md).

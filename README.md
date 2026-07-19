# Upload UI Evidence

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Upload%20UI%20Evidence-2f81f7?logo=github)](https://github.com/marketplace/actions/upload-ui-evidence)
[![Release](https://img.shields.io/github/v/release/mtzack-org/upload-ui-evidence)](https://github.com/mtzack-org/upload-ui-evidence/releases)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](LICENSE)

[日本語](README.ja.md)

Stop digging through CI artifacts after a UI test fails. Upload Playwright, Maestro, and Appium
screenshots, videos, HTML reports, traces, and logs to your private
[UI Evidence Portal](https://github.com/mtzack-org/ui-evidence-portal), then open the exact run from
the GitHub Actions Job Summary.

![UI Evidence Portal test run dashboard](docs/assets/portal-overview.png)

## Quick start

### 1. Deploy your private Portal

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fmtzack-org%2Fui-evidence-portal)

Connect a Vercel Private Blob store and complete the environment-variable setup in the
[Portal repository](https://github.com/mtzack-org/ui-evidence-portal).

### 2. Add one GitHub variable and one secret

- Repository variable `UI_EVIDENCE_PORTAL_URL`: your deployed Portal origin
- Repository secret `UI_EVIDENCE_INGEST_TOKEN`: the same value as the Portal's `EVIDENCE_INGEST_TOKEN`

### 3. Upload evidence after your UI tests run

```yaml
- name: Upload UI evidence
  if: always()
  uses: mtzack-org/upload-ui-evidence@v1
  with:
    portal-url: ${{ vars.UI_EVIDENCE_PORTAL_URL }}
    token: ${{ secrets.UI_EVIDENCE_INGEST_TOKEN }}
    platform: web
    status: ${{ job.status }}
    screenshots: test-results/**/*.png
    videos: test-results/**/*.webm
    reports: playwright-report/**/*.{html,zip}
    traces: test-results/**/*.zip
    logs: test-results/**/*.{log,txt,json}
    if-no-files-found: error
```

## Framework examples

| Test framework | Platforms | Workflow |
| --- | --- | --- |
| Playwright | Web and mobile browser emulation | [Playwright](examples/playwright.yml) |
| Maestro | Android and iOS; also React Native and Flutter | [Maestro](examples/maestro.yml) |
| Appium | Android and iOS native, hybrid, and mobile web | [Appium](examples/appium.yml) |

The [Playwright public demo](https://github.com/mtzack-org/upload-ui-evidence-playwright-demo)
runs end to end. The Maestro example expects your existing CI step to start a device and install the
app. The Appium example expects your runner/reporters to write evidence under `appium-results/`.

## What gets uploaded

| Input | Typical UI test output |
| --- | --- |
| `screenshots` | Failure and assertion screenshots |
| `videos` | `.webm` test recordings |
| `reports` | HTML reports or report archives |
| `traces` | Playwright traces and other trace files |
| `logs` | Text, JSON, and application logs |

Artifact inputs accept newline-separated glob patterns:

```yaml
screenshots: |
  test-results/**/*.png
  screenshots/**/*.jpg
```

The Action supports `web`, `android`, and `ios`. It exposes `run-id`, `run-url`, and
`uploaded-count` outputs. Optional result inputs are `total`, `passed`, `failed`, `skipped`, and
`duration-ms`.

When no files match, `if-no-files-found` controls whether the Action emits a warning (`warn`, the
default), fails (`error`), or remains silent (`ignore`). No empty Portal run is created.

For a Vercel Preview protected by Deployment Protection, pass an Automation Bypass secret through
`vercel-protection-bypass`. Do not set it for an unprotected production Portal.

## Development

Node.js 24 is required.

```bash
npm ci
npm test
npm run build
```

Commit `dist/` after changing the Action source. GitHub runners execute the committed bundle.

## Security and support

Read [`SECURITY.md`](SECURITY.md) before reporting a vulnerability. For reproducible bugs and feature
requests, follow [`SUPPORT.md`](SUPPORT.md).

## License

GNU Affero General Public License v3.0 only (`AGPL-3.0-only`). See [`LICENSE`](LICENSE).

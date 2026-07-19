# Upload UI Evidence

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Upload%20UI%20Evidence-2f81f7?logo=github)](https://github.com/marketplace/actions/upload-ui-evidence)
[![Release](https://img.shields.io/github/v/release/mtzack-org/upload-ui-evidence)](https://github.com/mtzack-org/upload-ui-evidence/releases)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](LICENSE)

[English](README.md)

UIテストが落ちるたびにCIのArtifactsを掘る作業をなくします。Playwright、Maestro、Appiumの
スクリーンショット、動画、HTMLレポート、Trace、ログを非公開の
[UI Evidence Portal](https://github.com/mtzack-org/ui-evidence-portal)へアップロードし、
GitHub ActionsのJob Summaryから該当runを直接開けます。

![UI Evidence Portalのテスト実行ダッシュボード](docs/assets/portal-overview.png)

## 60秒で導入

### 1. 非公開Portalをデプロイ

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fmtzack-org%2Fui-evidence-portal)

Vercel Private Blobストアを接続し、
[Portalリポジトリ](https://github.com/mtzack-org/ui-evidence-portal)の手順に沿って環境変数を設定します。

### 2. GitHub VariableとSecretを1つずつ追加

- Repository Variable `UI_EVIDENCE_PORTAL_URL`: デプロイしたPortalのオリジン
- Repository Secret `UI_EVIDENCE_INGEST_TOKEN`: Portal側の`EVIDENCE_INGEST_TOKEN`と同じ値

### 3. UIテスト実行後に証跡をアップロード

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

## フレームワーク別の例

| テストフレームワーク | 対象 | Workflow |
| --- | --- | --- |
| Playwright | Web、モバイルブラウザのエミュレーション | [Playwright](examples/playwright.yml) |
| Maestro | Android・iOS、React Native・Flutter | [Maestro](examples/maestro.yml) |
| Appium | Android・iOSのネイティブ、ハイブリッド、モバイルWeb | [Appium](examples/appium.yml) |

[Playwrightの公開デモ](https://github.com/mtzack-org/upload-ui-evidence-playwright-demo)は
E2Eで実行できます。Maestro例では既存のCI処理で端末起動とアプリのインストールを行ってください。
Appium例ではrunnerやreporterの出力先を`appium-results/`へ設定します。

## アップロードできるもの

| Input | UIテストでの主な用途 |
| --- | --- |
| `screenshots` | 失敗時やAssertion時のスクリーンショット |
| `videos` | `.webm`形式のテスト録画 |
| `reports` | HTMLレポートまたはそのアーカイブ |
| `traces` | Playwright TraceなどのTraceファイル |
| `logs` | テキスト、JSON、アプリケーションログ |

成果物の入力には、改行区切りのglobパターンを指定できます。

```yaml
screenshots: |
  test-results/**/*.png
  screenshots/**/*.jpg
```

対応プラットフォームは`web`、`android`、`ios`です。出力として`run-id`、`run-url`、
`uploaded-count`を利用できます。実行結果として`total`、`passed`、`failed`、`skipped`、
`duration-ms`も任意で指定できます。

一致するファイルがない場合の動作は`if-no-files-found`で指定します。既定値の`warn`は警告を表示し、
`error`はActionを失敗させ、`ignore`は何も表示しません。空のPortal runは作成されません。

Deployment Protectionで保護されたVercel Previewへ送信する場合は、Automation Bypassのシークレットを
`vercel-protection-bypass`へ渡してください。保護されていないProduction Portalでは設定しないでください。

## 開発

Node.js 24が必要です。

```bash
npm ci
npm test
npm run build
```

Actionのソースを変更した場合は`dist/`もコミットしてください。GitHub runnerはコミット済みのbundleを実行します。

## セキュリティとサポート

脆弱性を報告する前に[`SECURITY.md`](SECURITY.md)を確認してください。再現可能な不具合や機能要望は、
[`SUPPORT.md`](SUPPORT.md)の案内に従ってください。

## ライセンス

GNU Affero General Public License v3.0 only（`AGPL-3.0-only`）。詳しくは[`LICENSE`](LICENSE)を参照してください。

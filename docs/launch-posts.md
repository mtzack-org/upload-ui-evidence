# Launch posts

Ready-to-edit copy for announcing Upload UI Evidence. Replace the final sentence with a concrete
result or lesson after the first few external runs.

## Japanese — short

CIでE2Eが落ちるたびにArtifactsからスクショや動画を探すのが面倒だったので、Job Summaryから証跡を直接開けるGitHub Actionを作りました。

Playwright / Maestro / Appiumのscreenshots / videos / HTML reports / traces / logsに対応。PortalはVercelへ1クリックでセルフホストでき、コードはAGPLv3です。

Marketplace: https://github.com/marketplace/actions/upload-ui-evidence

## Japanese — Zenn / Qiita introduction

### タイトル

Playwrightが落ちたとき、Artifactsを掘らずにスクショ・動画・Traceを確認するGitHub Actionを作った

### 導入

PlaywrightをCIで動かしていると、失敗そのものより「どの証跡を見れば原因が分かるか」に時間を取られます。そこで、スクリーンショット、動画、HTMLレポート、Trace、ログを非公開Portalへまとめ、GitHub ActionsのJob Summaryから該当runへ直接移動できるActionを作りました。

この記事では、VercelへのPortal構築、GitHub Secretsの設定、workflowへの追加までを紹介します。

## English — short

I built a GitHub Action for the annoying part after a Playwright failure: finding the right screenshot, video, report, or trace in CI artifacts.

Upload UI Evidence sends them to a private, self-hosted Portal and adds the exact run link to the GitHub Actions Job Summary. The Portal deploys to Vercel, and the code is AGPLv3.

Marketplace: https://github.com/marketplace/actions/upload-ui-evidence

## English — Show HN / DEV title

Show HN: A GitHub Action that turns Playwright artifacts into a private test evidence portal

## English — Maestro community

### Title

Open-source GitHub Action for keeping Maestro screenshots, videos, reports, and logs in one portal

### Body

I built Upload UI Evidence for the debugging step after a mobile UI test fails in CI. It uploads
Maestro screenshots, recordings, HTML reports, and logs to a private self-hosted portal, then puts
the exact run link in the GitHub Actions Job Summary.

The repository now includes a Maestro workflow using `--test-output-dir` and `--debug-output`:
https://github.com/mtzack-org/upload-ui-evidence/blob/main/examples/maestro.yml

Marketplace: https://github.com/marketplace/actions/upload-ui-evidence

I would especially appreciate feedback on the artifact layout and what evidence your team needs
when a Maestro flow fails.

## English — Appium community

### Title

Open-source GitHub Action for collecting Appium screenshots, videos, reports, and logs

### Body

I built Upload UI Evidence to make failed mobile UI runs easier to inspect. It sends screenshots,
recordings, HTML reports, and logs from GitHub Actions to a private self-hosted portal and adds the
exact run link to the Job Summary.

The repository includes an Appium workflow template that works with any client or reporter writing
evidence to a shared directory:
https://github.com/mtzack-org/upload-ui-evidence/blob/main/examples/appium.yml

Marketplace: https://github.com/marketplace/actions/upload-ui-evidence

Feedback on common Appium artifact layouts and reporter combinations would be very useful.

# skill: bootstrap-astro

このリポジトリで AI 駆動 E2E を実行する前に必要なサーバ起動手順。

## 起動コマンド

```bash
pnpm install       # 初回 or 依存変更時のみ
pnpm build         # /dist/ を生成
pnpm preview       # → http://localhost:4321/ で配信開始
```

## 注意事項

- **ポートは 4321**。README に書かれている 3000 は古い情報。`astro.config.mjs` にポート指定が無いため `astro preview` のデフォルト 4321 が使われる。
- **`pnpm dev` ではなく `pnpm preview`** を使う。理由:
  - 本番ビルドと同じ静的出力で検証できる
  - HMR や画像の遅延生成が無く、スナップショットが安定する
- 起動中は `Local http://localhost:4321/` のログが出る。`curl -s http://localhost:4321/` が HTTP 200 を返すまで待つ。
- 既存の `@playwright/test` (`pnpm exec playwright test`) も `webServer` で 4321 を取りに行く可能性がある。**両者は同時に動かさない**。

## 既知のコンソールエラー (無視してよい)

トップページで以下の 404 が 3 件出るが、本プロジェクト未解決の既存問題で、AI E2E の合否には影響しない。期待値判定では「これらは無視」扱い。

```
/remark-github-alerts/styles/github-colors-light.css       404
/remark-github-alerts/styles/github-colors-dark-class.css  404
/remark-github-alerts/styles/github-base.css               404
```

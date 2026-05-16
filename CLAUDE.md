# CLAUDE.md

このリポジトリで Claude Code が作業する際の最小ガイド。

## プロジェクト概要

- Astro (static) + Tailwind + daisyUI で構築した研究室公式サイト
- パッケージマネージャ: `pnpm` (Node 22.14, `mise.toml` で固定)
- 主要コマンド: `pnpm dev` / `pnpm build` / `pnpm preview` / `pnpm check`
- 既存 E2E: `e2e/*.spec.ts` (Playwright Test, CI smoke 用)

## AI 駆動 E2E (このプロジェクトの目玉)

`@playwright/cli` (microsoft/playwright-cli) を Claude Code が直接叩いてブラウザ操作する仕組み。記事 [Claude Code with Playwright CLI (ZOZO Tech Blog)](https://techblog.zozo.com/entry/claude-code-with-playwright-cli) の OSS 実装版。

### ディレクトリ

```
e2e/ai/
  cases/       # 自然言語のテストケース (.md, git 管理)
  skills/      # 操作ルールやリポ固有手順 (.md, git 管理)
  reports/     # 実行結果レポート (.gitignore, 都度生成)
  screenshots/ # スクショ (.gitignore, 都度生成)
```

### 「ケースを実行して」と言われたら

1. **CLAUDE.md (このファイル) を Read**
2. **`e2e/ai/skills/*.md` を全部 Read** (bootstrap-astro / pc-session / report-format)
3. **対象ケース `e2e/ai/cases/<name>.md` を Read**
4. ユーザに以下を提示して**承認①**を得る
   - 叩く予定の `playwright-cli` コマンド列
   - 起動 URL とセッション名
   - preview サーバ起動状況の確認 (既起動 or `pnpm preview` を立てる必要)
5. 承認後、`playwright-cli -s=<name> open ...` から順に実行。各 click/goto の直後に snapshot を取り、状態を確認。
6. 失敗時は即停止しユーザに報告 → **承認②** (続行 / 中止)
7. 全終了後、`skills/report-format.md` の形式でレポート草案を提示 → **承認③** で `e2e/ai/reports/{YYYYMMDD-HHMM}-{caseId}.md` に保存。スクショは自動保存可。

### 守るべきこと

- **起動 URL は `http://localhost:4321/`** (README の 3000 は誤り。`astro preview` のデフォルト)
- `pnpm dev` ではなく **`pnpm preview` を使う** (HMR ・画像遅延生成を避ける)
- CSS セレクタ・XPath を直接書かない。**snapshot で得た `ref` を必ず経由**する
- 既存 `e2e/*.spec.ts` は触らない。改修依頼があったら別タスクとして扱う
- `e2e/ai/` と `pnpm exec playwright test` は同じポート (4321) を奪うので**同時実行禁止**
- スクショ・レポートを書き込む前に必ずユーザ承認を取る
- `@playwright/cli` のグローバル追加・削除はユーザの明示指示があるまで実行しない

### 既知のノイズ (無視してよい)

トップページで以下の 404 が 3 件出る。本プロジェクト既存の未解決問題で、AI E2E の合否には影響しない:

```
/remark-github-alerts/styles/github-colors-light.css       404
/remark-github-alerts/styles/github-colors-dark-class.css  404
/remark-github-alerts/styles/github-base.css               404
```

### スコープ (PoC 段階)

- 対象画面: `cases/top-page.md`, `cases/member-page.md` の 2 本
- デバイス: PC (Chrome) のみ。SP / WebKit は後フェーズ
- 自動化は未整備 (毎回 Claude への 1 行指示で起動)

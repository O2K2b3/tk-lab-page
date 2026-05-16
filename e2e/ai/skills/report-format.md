# skill: report-format

ケース実行後に `e2e/ai/reports/` に書く Markdown レポートの形式。

## ファイル名

`e2e/ai/reports/{YYYYMMDD-HHMM}-{caseId}.md`

例: `20260516-1430-top-page.md`

## テンプレート

```markdown
# Report: <ケース名>

- ケース: `e2e/ai/cases/<file>.md`
- 実行日時: YYYY-MM-DD HH:MM (JST)
- セッション名: <name>
- 起動 URL: http://localhost:4321/<path>
- 総合判定: ✅ PASS / ⚠️ PARTIAL / ❌ FAIL

## 実行ステップ

| # | 操作 | 期待 | 実測 | 判定 |
| --- | --- | --- | --- | --- |
| 1 | open / | title=高橋・狩川研究室 | title=高橋・狩川研究室 | ✅ |
| 2 | snapshot | nav に5リンク | 研究内容/メンバー/進路/ブログ/アクセス | ✅ |
| ... |

## 添付スクリーンショット

- `screenshots/<name>-01-top.png`
- `screenshots/<name>-04-member.png`

## 観測されたコンソール出力

- 既知の 404 (`/remark-github-alerts/styles/*.css`) 3 件 — 期待通り無視
- その他: なし

## NG / 気付き

- (なければ「なし」)
- 失敗があればステップ番号・推定原因・参考にした snapshot ファイルを列挙

## 次のアクション (提案)

- (修正 PR を切る / ケースを更新する / 何もしない、のいずれか)
```

## 書き方の注意

- 表は機械的に。装飾より「期待 vs 実測」が一目でわかることを優先。
- スクショは相対パス (`screenshots/...`) で参照。レポートと同じ `e2e/ai/` 起点。
- ユーザ承認を得てからファイル書き込み (草案を本文で提示してから保存)。

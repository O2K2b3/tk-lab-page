# skill: pc-session

`playwright-cli` のセッション運用ルール (PC 版)。

## セッション起動

```bash
playwright-cli -s=<name> open --browser=chrome http://localhost:4321/<path>
```

- `-s=<name>` でセッション名を付与。**ケース毎に別名**にして並列実行・終了管理を簡単にする。
- `--browser=chrome` で macOS の Chrome を使う (`playwright-cli install` 時に検出済み)。
- `open` 実行直後に snapshot が自動表示され、各要素に `ref=eN` が振られる。

## セッション名の命名規約

| ケース | セッション名 |
| --- | --- |
| `cases/top-page.md` | `top` |
| `cases/member-page.md` | `member` |
| その他 | ケースファイル名から `.md` を除き `-` を取った短縮形 |

## 基本コマンド

```bash
playwright-cli -s=<name> snapshot              # 全体のアクセシビリティツリー (ref 付与)
playwright-cli -s=<name> snapshot <ref>        # 部分ツリー (深い検証時)
playwright-cli -s=<name> snapshot --depth=N    # 浅く取得 (構造把握用)
playwright-cli -s=<name> click <ref>           # 要素クリック (CSS セレクタ不要)
playwright-cli -s=<name> fill <ref> "<text>"   # フォーム入力
playwright-cli -s=<name> screenshot --filename=e2e/ai/screenshots/<name>-<step>.png
playwright-cli -s=<name> goto <url>            # URL 遷移
playwright-cli -s=<name> go-back               # ブラウザバック
playwright-cli -s=<name> close                 # セッション終了
```

## 守るべきルール

- **ref ベースで操作する**。CSS セレクタや XPath を直接書かない。daisyUI / Tailwind の動的クラスは壊れやすい。
- **各 click/goto の直後に snapshot** を取り、状態を確認してから次の操作へ。ref はページ遷移で振り直される。
- スクショファイル名は `e2e/ai/screenshots/{session}-{step:02}-{label}.png` 形式。例: `top-04-member.png`。
- 終了時は必ず `close` を呼ぶ。残ると次回の `open` で衝突する。停滞時は `playwright-cli kill-all` で強制終了。

## 自動生成される成果物

- `.playwright-cli/page-<timestamp>.yml` — 各 snapshot の YAML (cwd 配下)
- `.playwright-cli/console-<timestamp>.log` — コンソールログ
- これらは `.gitignore` 済み。レポート作成時に必要な情報だけ `e2e/ai/reports/` に転記する。

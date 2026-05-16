# Case: top-page

トップページの基本表示とナビゲーション動作を確認する。

## 前提

- `skills/bootstrap-astro.md` の手順で http://localhost:4321/ が稼働中
- セッション名: `top`
- デバイス: PC (Chrome, viewport デフォルト)

## 手順

1. `playwright-cli -s=top open --browser=chrome http://localhost:4321/` でブラウザを開き、初回スナップショットを取得する
2. snapshot を確認し、`navigation` 内の `list` に以下 5 リンクが存在することを確認する
   - 研究内容 (`/research/`)
   - メンバー (`/member/`)
   - 進路 (`/alumni/`)
   - ブログ (`/blogs/`)
   - アクセス (`/contact/`)
3. main の最初のセクションに「都市の画像」とキャッチコピー「人間と機械の望ましい関係を目指して」が含まれることを確認する
4. 中段のフィーチャーカード3つ (「研究内容」「ニュース」「特徴」) と heading「幅広い分野で研究を行っています」が見えることを確認する
5. ページ下部に「News & Blog」セクションがあり、少なくとも 1 件以上の `link` (ニュース記事) が並ぶことを確認する
6. `playwright-cli -s=top screenshot --filename=e2e/ai/screenshots/top-01-top.png --full-page` でスクショ保存
7. ナビゲーションの「メンバー」リンク (snapshot で得た ref) を click し、URL が `/member/` に、title が「メンバー」に変わることを確認する
8. `playwright-cli -s=top screenshot --filename=e2e/ai/screenshots/top-07-after-member-nav.png` を保存
9. `playwright-cli -s=top close` でセッション終了

## 期待値

- 上記 nav 5 リンクが**順序通り**揃っている
- 各ヒーロー/フィーチャー/News&Blog セクションが描画されている
- 「メンバー」クリックで `/member/` に遷移し、title が「メンバー」になる
- スクショ 2 枚が `screenshots/` に保存される
- コンソールエラーは `bootstrap-astro.md` の既知 404 (3 件) のみ。新規エラーがあれば NG として記録

## NG 時の扱い

- 失敗ステップ番号と、観測した snapshot ファイルパス、推定原因を `reports/{timestamp}-top-page.md` に記録
- 自動修正は行わない (人間レビュー前提)
- 重大エラー (ブラウザクラッシュ / preview サーバ停止 / 想定外の例外) は即時停止しユーザに報告

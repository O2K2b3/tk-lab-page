# Case: member-page

メンバー紹介ページの構成 (スタッフ・学生グリッド) を確認する。

## 前提

- `skills/bootstrap-astro.md` の手順で http://localhost:4321/ が稼働中
- セッション名: `member`
- デバイス: PC (Chrome, viewport デフォルト)

## 手順

1. `playwright-cli -s=member open --browser=chrome http://localhost:4321/member/` でブラウザを開き、初回スナップショットを取得する
2. main 内に `heading "メンバー紹介" [level=1]` が存在することを確認する
3. main 配下の最初の `generic` (スタッフグリッド) に少なくとも 4 件の `link` があり、それぞれリンク先が `staff/<name>/` 形式であることを確認する (例: 高橋 信 / 狩川 大輔 / 堀内 友翔 / 鯨井 恵理)
4. main 配下の次の `generic` (学生グリッド) に少なくとも 1 件の `link` があり、リンク先が `student/doctor/<name>/` または `student/<grade>/<name>/` 形式であることを確認する
5. `playwright-cli -s=member screenshot --filename=e2e/ai/screenshots/member-01-grid.png --full-page` でスクショ保存
6. スタッフ最初のリンク (snapshot で得た ref) を click し、URL が `staff/<name>/` を含むこと、title が当該人物名を含むことを確認する
7. `playwright-cli -s=member screenshot --filename=e2e/ai/screenshots/member-06-detail.png` を保存
8. `playwright-cli -s=member go-back` で一覧に戻れることを確認する
9. `playwright-cli -s=member close` でセッション終了

## 期待値

- h1「メンバー紹介」が存在
- スタッフ 4 名以上、学生 1 名以上のリンクが描画されている
- スタッフ詳細ページに遷移でき、戻れる
- スクショ 2 枚が `screenshots/` に保存される
- コンソールエラーは既知 404 のみ

## NG 時の扱い

- `cases/top-page.md` と同様
- 「メンバー数が事前期待より大幅に少ない」場合は data ソース (`src/content/` または `src/data/`) の取り込み不全を疑う旨をレポートに記載

# 開発方針＆開発環境ルール(maidcafe-programming-school)

この節は[`open-raid-z`](https://github.com/aon-co-jp/open-raid-z)の`CLAUDE.md`を
**正本**とし、各プロジェクトへコピーして同期する既存の運用ルール継承方針に準じる
(全リポジトリ共通ルールの詳細はここに複製せず`open-raid-z/CLAUDE.md`を参照すること)。

## このリポジトリの役割

[open-english](https://github.com/aon-co-jp/open-english)と連携する、
「プログラミング × 語学学習」の実践スクール構想と、そこで使うカリキュラム資料
(`curriculum/`)をまとめるリポジトリ。**このリポジトリ自体はアプリケーション
コードを持たない**——実際の学習体験(チャットでの対話・コード表示・言語切り替え・
AI先生プログラミング講座)は、open-english本体(`web/app.js`の
`teachProgrammingTopic`/`suggestCollaborativeDevPlan`関連)に実装されている。

詳細は[README.md](README.md)を参照。

## HANDOFF

- **2026-09-29 新設**: ユーザー指示(「Maid Cafe Programming School」構想、
  open-english + aruaru-search/aruaru-llm連携、コーセラのデータサイエンティスト
  育成プログラムを参考にしたカリキュラム、ニュース/ブログ/URL/フリーランス案件を
  題材にした相談型開発(希望者のみ)+多言語学習)を受けて新規作成。

  **実装した内容(open-english本体側)**:
  1. `curriculum/data-science-path.md`/`.json`: コーセラの主要プログラム
     (Google データアナリティクス、IBM データサイエンス、米大学オンライン学位)と、
     データサイエンスチームの実務3領域(インサイト業務・プロダクト実験・
     アナリティクス有効化)を構造化データとして整理。open-englishのAI先生機能が
     将来参照する想定(現時点では未接続)。
  2. `curriculum/collaborative-dev-concept.md`: 相談型開発機能の設計ドキュメント。
  3. **設計を元に、open-english本体(`web/app.js`)へ実際に実装済み**:
     `isCollaborativeDevRequest`/`suggestCollaborativeDevPlan`。ニュース記事・
     フリーランス案件等の長文またはURL+「一緒に開発したい」等の明示的な意思表示
     の両方が揃ったときのみ発火(誤検知防止の二重条件、既存の
     `isProgrammingLearnRequest`と同じ設計方針)。ブラウザのCORS制約により
     URL先の全文取得はできないため、その旨を正直に伝えた上で本文の貼り付けを
     依頼する設計。実機確認済み(Rust案件テキスト+URLで正しく発火、
     意思表示の無い長文では発火しないことを確認)。

  **未接続(次回以降)**: `curriculum/data-science-path.json`をopen-englishの
  AI先生機能から実際に参照する接続は未実装(現状はこのリポジトリ内の資料のみ)。

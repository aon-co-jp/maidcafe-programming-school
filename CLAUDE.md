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

- **2026-09-30続き2 rust-poemコース内でTauriも実際に学べるように**: ユーザー
  指示「Rust+PoemでもRPeomでもコース内容的にはTauriの内容も学べるようにして」
  への対応。単なるメリット欄への言及に留めず、`rust-poem`スタックへ
  `desktopSnippet`(Tauriの`#[tauri::command]`実装例、Poem/RPoemサーバーへ
  HTTPで問い合わせる構成)と`desktopNoteJa`/`desktopNoteEn`を追加。
  open-english側`teachWebDevStack`(`web/app.js`)も、`stack.desktopSnippet`が
  存在する場合に「デスクトップアプリ化: Tauri」のセクションとコード例を
  追加表示するよう対応。実機テストで「RPoemを学びたい」「Tauriを学びたい」
  のどちらでも同じコース内でバックエンド(Poem/RPoem)+デスクトップ化
  (Tauri)の両方のコード例が表示されることを確認済み。

- **2026-09-30続き 「Rust + Tauri + Poem/RPoem」独立スタックを撤回**: 一度
  4つ目のスタックとして追加したが、ユーザー指摘「RPoemはTauriが含まれていた
  ので除去」を受けて撤回。RPoem自体に既にTauri連携が含まれているとのことで、
  重複を避けるため独立スタックにはせず、既存の`rust-poem`スタックの
  `aliases`に`"tauri"`を追加、`prosJa`/`prosEn`に「RPoemはTauri連携も含んで
  おりデスクトップアプリ化にも使える」旨を追記する形に統一した。3スタック
  構成(PHP+Laravel/Python+FastAPI/Rust+Poem・RPoem)に戻っている。

- **2026-09-30 データサイエンス接続の完了+基本的なWEBサイト開発コース新設**:
  ユーザー指示(「データサイエンティストになりたい」の自動発火説明+
  「PHP + LARAVELコースと、Python＋FastAPIコースと、Rust＋PoemかRPoemコースで、
  +aruaru-db ＋HTML5+CSS3＋TypeScriptなどで基本的なWEBサイトの開発を学習する
  コースを新設して」)への対応。
  1. `curriculum/web-dev-path.json`新設(正本)。PHP+Laravel/Python+FastAPI/
     Rust+Poem・RPoemの3スタック、共通フロントエンド(HTML5/CSS3/TypeScript)+
     aruaru-db(GraphQL、APIキー自動発行)。open-english側(`web/web-dev-path.json`、
     複製)から`fetch`して使う構成(データサイエンスコースと同じ方式)。
  2. **設計変更**: 当初`data-science-path.json`はJS側にハードコードされた配列
     (`WEB_DEV_STACKS`)として実装したが、「maidcafe-programming-schoolと連携して」
     との指示を受け、検出用の軽量なalias表(`WEB_DEV_STACK_ALIASES`、キーと
     aliasesのみ)だけをJS側に残し、表示内容(スニペット・メリデメ)は全て
     `web-dev-path.json`を`fetch`して取得する方式へリファクタリングした
     (`fetchWebDevPath`、`fetchDataSciencePath`と同じパターン)。
  3. **誤検知防止**: 「Rust + Poem」の`Poem`は一般的な英単語(詩)でもあるため、
     `rpoem`単体のalias一致に加え、`poem`と`rust`が両方含まれる場合のみ
     Rust+Poemコースと判定する特別ルールを追加。実機テストで「この詩(poem)を
     学びたい」が誤検知しないこと、「Rust + Poemを学びたい」が正しく検出される
     こと、スタック名無しの「webサイト開発を学びたい」が3択の概要を案内する
     ことを確認済み。

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

## 音声・音質の研究の成果(maid-cafe-se由来、2026-09-30)

ユーザー指示(2026-09-30)「この音質研究はaruaru-llmにもopen-englishにもmaidcafe-programming-schoolにもmake-diskにもmaid-cafe-seにも影響させて」に基づく。
正本は[`aon-co-jp/maid-cafe-se`](https://github.com/aon-co-jp/maid-cafe-se)の`PORTING.md`「音質向上の研究」節。ここには、このリポジトリに関係する要点だけを書く。

| 項目 | 結果(すべて実測。聴感ではなく数値・テストでの検証) |
|---|---|
| 音程と声の太さ(フォルマント)を独立に制御 | リサンプリング方式は音程を動かすと声の太さも同じ比率で動く(音程を下げると「怪物っぽい声」)。FFT+ケプストラム包絡の周波数伸縮補正で独立に制御できた。直接合成した正解の母音との包絡距離: 新方式1.6dB、旧方式10.3dB(音程0.72倍・声の太さ据え置き)。補正ゲイン上限は±12dBだと鋭いフォルマントを動かせず、±24dBで解決 |
| AI帯域拡張(LavaSR、Apache-2.0、学習データVCTK) | make-diskの設計(入力の帯域は変えず、高域だけを頭打ちつきで足す)が声にも有効。ただし**声は高域が「崖」でなくなだらかに減衰する**ため、音楽向けの崖検出は12.4kHzを返し可聴域に何も足さなかった。声向けのロールオフ検出を新設。Windows音声Harukaで7.5kHzを検出、自己教師あり評価(6kHzで帯域制限→拡張→元の音声とのLSD、6〜9.5kHz)46.8dB→12.9dB、入力の帯域は変化なし。**LSDはスペクトル包絡の近さで聴感品質ではない。拡張前は帯域が無音のため差の大部分は「何か入れれば縮む」分** |
| 日本語ニューラルTTS | 安全に配布アプリへ同梱できるモデルは未発見。sherpa-onnx公式に日本語TTSモデルは無い/piper-plus系は日本語の学習データがMOE-Speech(ゲーム音声、機械学習解析目的のみ・再配布禁止)由来で配布不可/Kokoro日本語は作者評価がC+〜C-でG2Pの移植が重い |
| Rust化と音質 | Rust化そのものは音質を変えない。Kotlin版とRust版の出力は数値的に同一(最大誤差0.00000)、速度もウォーム時はほぼ同じ(4秒の音声を、Kotlin 36ms/126ms、Rust 34ms/68ms、単独/ハモり) |

実装: `maid-cafe-se`の`crates/maid-cafe-core/src/audio/`(依存クレート無しの純Rust。wasm32-unknown-unknown向けのコンパイルは確認済み、ブラウザでの実行は未検証)と`crates/maid-cafe-enhance`(tract+ONNX、モデル約56MBは固定リビジョン+SHA-256で取得/同梱)。

### このリポジトリへの影響

- **コード変更なし**。このリポジトリは設計資料とカリキュラムだけでコードを持たず(README参照)、AI先生の画面・声はopen-english側にある。したがって声質の実装はopen-englishの版ごと(WEB版・ローカル版・ミックス版)の対応表に従う(`open-english/CLAUDE.md`の同名節)。
- カリキュラム(`curriculum/`)の文章が声で読み上げられる場合の注意: 読み上げでは、記号・コードブロック・URLの読み上げが不自然になる。声で読ませる文は短く、記号を最小にする(`maid-cafe-core`の`SpeechText`の方針)。

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

実装: **RPoemの共有クレート`open-runo-voice`**(`RPoem/crates/open-runo-voice`、`docs/voice.md`。依存クレート無しの純Rust。wasm32-unknown-unknown向けのコンパイルは確認済み、ブラウザでの実行は未検証)。AI帯域拡張は`maid-cafe-se`の`crates/maid-cafe-enhance`(tract+ONNX、モデル約56MBは固定リビジョン+SHA-256で取得/同梱。重い依存をRPoem本体へ持ち込まないため別置き)。

### このリポジトリへの影響

- **コード変更なし**。このリポジトリは設計資料とカリキュラムだけでコードを持たず(README参照)、AI先生の画面・声はopen-english側にある。したがって声質の実装はopen-englishの版ごと(WEB版・ローカル版・ミックス版)の対応表に従う(`open-english/CLAUDE.md`の同名節)。
- カリキュラム(`curriculum/`)の文章が声で読み上げられる場合の注意: 読み上げでは、記号・コードブロック・URLの読み上げが不自然になる。声で読ませる文は短く、記号を最小にする(`maid-cafe-core`の`SpeechText`の方針)。

## API 方針: REST 全廃・gRPC / Connect へ (2026-11-04)

### 日本語
- **REST は全廃する。新しい API は REST で作らない。** ブラウザや `curl` から呼ぶ API は **Connect の単純呼び出し**(`POST /<パッケージ>.<Service>/<Method>`、`content-type: application/json`、本文は JSON)、サービス間は **gRPC / gRPC-web**(RPoem の管理API `open_runo.admin.v1.Admin` と同じ流儀)。ストリーミングは、必要になってから。
- **既存の REST は、触るときに順に移す。** 移したら旧 REST の経路は消す。移行が終わるまで残るが、**新規には追加しない**。
- **JSON は RS-JSON に統一**する(読み込みは `from_str_secure` / `parse_secure` でサイズ・深さ・重複キーの上限つき、書き出しは `rust_json::fast`)。ブラウザの WASM に `prost` は入れない(サイズが増えるため)。
- **例外(自分たちの API ではなく、相手の規格が決めているもの)**: ACME、OAuth/OIDC のコールバック、第三者(GitHub など)からの Webhook、ヘルスチェック、静的ファイル配信、OTLP など。これらは規格どおりの HTTP を受ける。
- 参考実装: `RPoem`(`open_runo.admin.v1.Admin`)、`open-easy-web`(`server/src/connect.rs`、`proto/open_easy_web/admin/v1/admin.proto`)。
- ※ このリポジトリのコードが既に方針どおりかどうかは、各リポジトリの現状による。この節は方針の記録であり、コードの変更を意味しない。

### English
- **REST is being abolished. New APIs are not built as REST.** APIs called from browsers or `curl` use **Connect unary calls** (`POST /<package>.<Service>/<Method>`, `content-type: application/json`, JSON body); service-to-service calls use **gRPC / gRPC-web** (the same style as RPoem's admin API `open_runo.admin.v1.Admin`). Streaming only when it is actually needed.
- **Existing REST endpoints are migrated one by one, when they are touched.** The old REST route is removed once migrated. Until migration is finished they remain, but **no new ones are added**.
- **JSON is unified on RS-JSON**: reading with `from_str_secure` / `parse_secure` (size, depth and duplicate-key limits), writing with `rust_json::fast`. `prost` is not put into browser WASM (it would grow the binary).
- **Exceptions (shapes dictated by someone else's standard, not our own API)**: ACME, OAuth/OIDC callbacks, webhooks from third parties (e.g. GitHub), health checks, static file serving, OTLP, and so on. These accept plain HTTP as the standard requires.
- Reference implementations: `RPoem` (`open_runo.admin.v1.Admin`) and `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Note: whether the code in a given repository already follows this policy depends on that repository. This section records the policy; it does not mean the code has been changed.

### Deutsch (KI-Übersetzung, noch nicht von Muttersprachlern geprüft)
- **REST wird abgeschafft. Neue APIs werden nicht als REST gebaut.** APIs, die aus dem Browser oder mit `curl` aufgerufen werden, nutzen **Connect-Unary-Aufrufe** (`POST /<Paket>.<Service>/<Methode>`, `content-type: application/json`, JSON-Body); zwischen Diensten gilt **gRPC / gRPC-web** (wie bei der Admin-API von RPoem, `open_runo.admin.v1.Admin`). Streaming erst, wenn es wirklich nötig ist.
- **Bestehende REST-Endpunkte werden einzeln migriert, sobald man sie anfasst.** Nach der Migration wird die alte REST-Route entfernt. Bis dahin bleiben sie bestehen, es werden aber **keine neuen hinzugefügt**.
- **JSON wird auf RS-JSON vereinheitlicht**: Lesen mit `from_str_secure` / `parse_secure` (Grenzen für Größe, Tiefe und doppelte Schlüssel), Schreiben mit `rust_json::fast`. `prost` kommt nicht in Browser-WASM (die Datei würde größer).
- **Ausnahmen (Formate, die ein fremder Standard vorgibt, nicht unsere eigene API)**: ACME, OAuth/OIDC-Callbacks, Webhooks von Dritten (z. B. GitHub), Health-Checks, statische Dateien, OTLP usw. Diese nehmen schlichtes HTTP entgegen, wie es der Standard verlangt.
- Referenzimplementierungen: `RPoem` (`open_runo.admin.v1.Admin`) und `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Hinweis: Ob der Code eines Repositorys diese Richtlinie schon befolgt, hängt vom jeweiligen Repository ab. Dieser Abschnitt hält die Richtlinie fest; er bedeutet nicht, dass der Code bereits geändert wurde.

### Русский (машинный перевод, носителями языка ещё не проверен)
- **REST полностью отменяется. Новые API не создаются как REST.** API, вызываемые из браузера или через `curl`, используют **унарные вызовы Connect** (`POST /<пакет>.<Service>/<Method>`, `content-type: application/json`, тело — JSON); между сервисами — **gRPC / gRPC-web** (так же, как административный API RPoem, `open_runo.admin.v1.Admin`). Потоковая передача — только когда она действительно нужна.
- **Существующие REST-эндпоинты переносятся по одному, когда до них доходят руки.** После переноса старый REST-маршрут удаляется. До завершения переноса они остаются, но **новые не добавляются**.
- **JSON унифицируется на RS-JSON**: чтение через `from_str_secure` / `parse_secure` (ограничения на размер, глубину и повторяющиеся ключи), запись через `rust_json::fast`. `prost` не добавляется в браузерный WASM (размер вырос бы).
- **Исключения (форматы, заданные чужим стандартом, а не наш собственный API)**: ACME, обратные вызовы OAuth/OIDC, вебхуки сторонних сервисов (например, GitHub), проверки работоспособности, раздача статических файлов, OTLP и т. п. Они принимают обычный HTTP, как требует стандарт.
- Эталонные реализации: `RPoem` (`open_runo.admin.v1.Admin`) и `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Примечание: соответствует ли код конкретного репозитория этой политике, зависит от самого репозитория. Этот раздел фиксирует политику и не означает, что код уже изменён.

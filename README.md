# Maid Cafe Programming School

*This repository documents a design concept and a small curated curriculum. It has no application code of its own — the actual teaching UI lives in [open-english](https://github.com/aon-co-jp/open-english)'s chat (see "How this connects to open-english" below). English follows the Japanese below.*

## これは何か

[open-english](https://github.com/aon-co-jp/open-english)(AI活用の語学学習プラットフォーム)と連携する、「プログラミング × 語学学習」を同時に進める実践スクールの構想と、そこで使うカリキュラム資料をまとめたリポジトリです。

秋葉原メイドカフェの接客技法・英会話研修を参考にしたopen-englishの世界観を引き継ぎ、「先生(AI)」と「生徒(利用者)」が一緒に手を動かしながら、プログラミングと言語(日本語・英語・その他130言語)の両方を学べることを目指します。

**正直な開示: このリポジトリ自体はコードを持ちません。** 実際の学習体験(チャットでの対話・コード表示・言語切り替え)は、既にopen-english本体に実装済みの機能(下記)がそのまま担います。このリポジトリは、その機能が参照する**カリキュラム資料**(`curriculum/`)と、**設計方針**をまとめる場所です。

## open-englishとの連携(既に実装済みの部分)

open-englishのチャットには、2026-09-29時点で以下がすでに実装されています(`web/app.js`の`teachProgrammingTopic`関連)。

- チャットで「Pythonを学びたい」のように言語/フレームワーク名+学習意図を書くと、AI先生が変数・クラス・for文・代表的なアルゴリズム10種・グローバル変数を避ける理由(メリデメ)・言語自体のメリデメを解説
- 内容は小型AIモデル(aruaru-llm)の生成に頼らず、人手で確認済みの固定テキストを使用(事実性を優先)
- Google検索(設定済みの場合)で、公式サイト・入門ブログ・GitHubへの参考リンクを安全な許可リスト方式で提示
- VS Code companion拡張機能(`open-english`リポジトリの`vscode-extension/`)には、AI先生/生徒の色分け編集ビジュアライザとLive Share同梱も用意済み

このリポジトリのカリキュラム資料(`curriculum/`)は、open-englishのAI先生機能から実際に`fetch`されて使われています(データサイエンティストコース・基本的なWEBサイト開発コースの両方、2026-09-30時点で接続済み)。

## AIの選択肢(利用者が選ぶ、無料〜有料)

open-englishと組み合わせて使うAIは、利用者が用途と予算に応じて選べます。

| 選択肢 | 費用 | 特徴 |
|---|---|---|
| [aruaru-search](https://github.com/aon-co-jp/aruaru-search) + [aruaru-llm](https://github.com/aon-co-jp/aruaru-llm) | 完全無料 | 自前ホスト、小型モデル(GPT-2級)のため応答品質は限定的 |
| ChatGPT / DeepSeek / Grok | 無料お試し枠あり | 各社の無料枠内で利用 |
| Claude Code Desktop | 有料 | 現時点で最も高性能、複雑な開発相談・大規模なコード生成向け |

## コーセラを参考にしたデータサイエンティスト育成カリキュラム

アメリカで求められるデータサイエンティスト育成の実例として、[Coursera](https://www.coursera.org)の主要プログラムを参考に整理しました(詳細は[curriculum/data-science-path.md](curriculum/data-science-path.md)、構造化データは[curriculum/data-science-path.json](curriculum/data-science-path.json))。

- **Google データアナリティクス プロフェッショナル認定**: 初心者向け、SQL・R・Tableau等の基礎
- **IBM データサイエンス プロフェッショナル認定**: Python・機械学習・SQLの実践スキル
- **アメリカの大学のオンライン学位**(例: コロラド大学ボルダー校 Master of Science in Data Science)
- **実務の3領域**(コーセラ社内データサイエンティームの例を参考): (1) インサイト業務(因果推論・統計) (2) プロダクト実験(実験デザイン・分析) (3) アナリティクス有効化(データパイプライン・ダッシュボード構築)

これらを題材に、語学学習・プログラミング学習・スマホアプリ/WEBサイト開発を並行して楽しめることを目指します。

## 基本的なWEBサイト開発コース

3つのバックエンドスタックから選べる、実践的なWEBサイト開発コースです(詳細は[curriculum/web-dev-path.json](curriculum/web-dev-path.json))。

| スタック | 特徴 |
|---|---|
| PHP + [Laravel](https://laravel.com) | 世界中のレンタルサーバーで動くPHP+フルスタックフレームワーク。学習リソースが豊富 |
| Python + [FastAPI](https://fastapi.tiangolo.com) | 型ヒントからAPI仕様書を自動生成、非同期処理で高速。Pythonのライブラリ資産と連携しやすい |
| Rust + Poem / [RPoem](https://github.com/aon-co-jp/RPoem) | 型安全・高速。aon-co-jpのaruaru-llm/aruaru-db等の多くがRPoem土台なので他リポジトリが参考になる。[Tauri](https://tauri.app)連携も含んでおり、同じバックエンドをデスクトップアプリ化する際にも使える |

いずれのスタックでも、フロントエンドはHTML5+CSS3+TypeScript、データベースは[aruaru-db](https://github.com/aon-co-jp/aruaru-db)(GraphQL、APIキー自動発行)で共通です。open-englishのチャットで「Laravelを学びたい」のようにスタック名+学習意図を書くと自動発火し、スタック名を書かず「webサイト開発を学びたい」と書くと3択の概要が案内されます。

## ニュース・ブログ・URL・フリーランス案件を題材にした相談型開発(希望者のみ)

**あくまでもユーザーが希望した場合のみ**、インターネットニュース・ブログ記事・利用者が提示したURL・フリーランス案件の内容を題材に、「一緒にスマホアプリやWEBサイトを開発してみる」相談ができる機能を想定しています。その過程で、選択中の言語(または英語・日本語・チャットで書いた言語)でのやり取りを通じて、プログラミングと語学の両方を同時に学べるようにします。

**現在のスコープ(正直な開示)**: この機能はまだ設計段階で、open-english本体への実装は未着手です。理由は主に2つです。

1. ブラウザからは任意サイトのURLを直接クローリング(全文取得)できません(CORS制約)。ユーザーが本文をチャットに貼り付ける、または既存のGoogle検索機能で要約を取得する形が現実的です。
2. 「一緒に開発する」を安全に(=詐欺サイトへの誘導や、著作権を侵害するコード生成をしない形で)実現するには、既存の`isSafeLinkDomain`/`buildSafeResultLink`の許可リスト方式や、著作権遵守の方針を踏まえた設計が必要です。

次のマイルストーンとして、open-english側に「ユーザーが本文を貼り付け、開発したいという意思表示をしたときだけ」発火する、オプトインの企画支援チャット機能を追加する予定です(詳細は[curriculum/collaborative-dev-concept.md](curriculum/collaborative-dev-concept.md))。

## 関連プロジェクト

- [open-english](https://github.com/aon-co-jp/open-english) — 本プロジェクトが連携する語学学習プラットフォーム本体
- [aruaru-search](https://github.com/aon-co-jp/aruaru-search) / [aruaru-llm](https://github.com/aon-co-jp/aruaru-llm) — 完全無料のAI検索・AI基盤
- [aon-co-jp on GitHub](https://github.com/aon-co-jp) — 開発組織

---

## English

A design concept and curated curriculum for combining programming education with language learning, as a companion to [open-english](https://github.com/aon-co-jp/open-english)'s chat. **This repository has no application code of its own** — open-english's chat already implements the AI-teacher UI (see "How this connects to open-english" above for what's live today).

The curriculum materials here (`curriculum/`) summarize a Coursera-inspired data science learning path (Google Data Analytics Professional Certificate, IBM Data Science Professional Certificate, university online degrees, and the three core work areas of a data science team: insight work, product experimentation, analytics enablement), intended to eventually be referenced by open-english's AI-teacher feature (not yet wired up — see "Current scope" above for the honest status).

A separate, opt-in feature is planned: letting a user paste a news article, blog post, URL, or freelance job posting and — **only if they choose to** — get help planning a small app/website together with the AI teacher, practicing whichever language they're chatting in along the way. This is design-only for now; see [curriculum/collaborative-dev-concept.md](curriculum/collaborative-dev-concept.md) for the plan and the honest reasons it isn't implemented yet (browser CORS limits on fetching arbitrary URLs, and the safety design needed before shipping it).

---

作者 / Author: 石 塚 正 浩（MASAHIRO ISHIZUKA） (https://github.com/aon-co-jp)

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

### Deutsch (KI-Übersetzung, redaktionell überarbeitet; noch nicht von Muttersprachlern geprüft)
- **REST wird vollständig abgeschafft; neue APIs entstehen nicht mehr als REST.** APIs, die aus dem Browser oder per `curl` aufgerufen werden, verwenden **unäre Connect-Aufrufe** (`POST /<Paket>.<Service>/<Methode>`, `content-type: application/json`, JSON-Body). Zwischen Diensten kommen **gRPC bzw. gRPC-web** zum Einsatz, im selben Stil wie die Admin-API von RPoem (`open_runo.admin.v1.Admin`). Streaming wird erst ergänzt, wenn es tatsächlich gebraucht wird.
- **Bestehende REST-Endpunkte werden schrittweise umgestellt, und zwar immer dann, wenn man ohnehin an ihnen arbeitet.** Sobald ein Endpunkt umgestellt ist, wird die alte REST-Route entfernt. Bis zum Abschluss der Migration bleiben die übrigen bestehen, es werden aber **keine neuen mehr hinzugefügt**.
- **JSON wird einheitlich über RS-JSON verarbeitet**: Gelesen wird mit `from_str_secure` bzw. `parse_secure` (mit Obergrenzen für Größe, Verschachtelungstiefe und doppelte Schlüssel), geschrieben mit `rust_json::fast`. `prost` wird nicht in das Browser-WASM aufgenommen, da es die Datei vergrößern würde.
- **Ausnahmen (Schnittstellen, deren Form ein fremder Standard vorgibt und die nicht zu unserer eigenen API gehören)**: ACME, OAuth/OIDC-Callbacks, Webhooks von Drittanbietern (z. B. GitHub), Health-Checks, die Auslieferung statischer Dateien, OTLP und Ähnliches. Diese nehmen, wie vom jeweiligen Standard verlangt, gewöhnliches HTTP entgegen.
- Referenzimplementierungen: `RPoem` (`open_runo.admin.v1.Admin`) und `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Hinweis: Ob der Code eines einzelnen Repositorys diese Richtlinie bereits erfüllt, hängt vom jeweiligen Repository ab. Dieser Abschnitt dokumentiert lediglich die Richtlinie; er besagt nicht, dass der Code schon angepasst wurde.

### Русский (перевод выполнен с помощью ИИ и отредактирован; носителями языка пока не проверен)
- **От REST мы полностью отказываемся; новые API больше не создаются в стиле REST.** API, к которым обращаются из браузера или через `curl`, используют **унарные вызовы Connect** (`POST /<пакет>.<Service>/<Method>`, `content-type: application/json`, тело запроса — JSON). Для взаимодействия между сервисами применяются **gRPC и gRPC-web** — в том же стиле, что и административный API RPoem (`open_runo.admin.v1.Admin`). Потоковая передача данных добавляется лишь тогда, когда в ней действительно возникает необходимость.
- **Существующие REST-эндпоинты переводятся поэтапно — по мере того, как с ними приходится работать.** После перевода старый REST-маршрут удаляется. Пока миграция не завершена, оставшиеся эндпоинты сохраняются, но **новые не добавляются**.
- **Работа с JSON унифицирована на основе RS-JSON**: чтение — через `from_str_secure` или `parse_secure` (с ограничениями на размер, глубину вложенности и повторяющиеся ключи), запись — через `rust_json::fast`. `prost` в браузерный WASM не включается, поскольку это увеличило бы размер файла.
- **Исключения (интерфейсы, форму которых определяет чужой стандарт, а не наш собственный API)**: ACME, обратные вызовы OAuth/OIDC, вебхуки сторонних сервисов (например, GitHub), проверки работоспособности, раздача статических файлов, OTLP и тому подобное. Такие интерфейсы принимают обычный HTTP, как того требует соответствующий стандарт.
- Эталонные реализации: `RPoem` (`open_runo.admin.v1.Admin`) и `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Примечание: соответствует ли код конкретного репозитория этой политике, зависит от самого репозитория. Данный раздел лишь фиксирует политику и не означает, что код уже изменён.

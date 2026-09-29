# フロントエンド開発規約

## ディレクトリ構成

### tree構成

```
my-app/
├── public/
│   └── images/
│
├── src/
│   ├── api/
│   │   └── openapi/
│   │       └── generated/
│   │           ├── index.ts
│   │           ├── model/
│   │           │   └── userResponse.ts
│   │           ├── users/
│   │           │   └── users.ts
│   │           └── products/
│   │               └── products.ts
│   │
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── globals.css
│   │   │
│   │   ├── dashboard/
│   │   │   ├── page.tsx
│   │   │   ├── layout.tsx
│   │   │   ├── _components/
│   │   │   │   └── DataCard.tsx
│   │   │   ├── _lib/
│   │   │   │   ├── _services/
│   │   │   │   │   └── getDashboardDataClient.ts
│   │   │   │   ├── _hooks/
│   │   │   │   │   └── useDashboardData.ts
│   │   │   │   └── _schemas/
│   │   │   │       └── schema.ts
│   │   │   └── _types/
│   │   │       └── index.ts
│   │   │
│   │   └── sample/
│   │       ├── page.tsx
│   │       ├── _components/
│   │       │   └── SampleForm.tsx
│   │       ├── _lib/
│   │       │   ├── _services/
│   │       │   │   ├── getSampleItemsServer.ts
│   │       │   │   └── updateSampleItemClient.ts
│   │       │   ├── _hooks/
│   │       │   │   └── useSampleData.ts
│   │       │   ├── _schemas/
│   │       │   │   └── formSchema.ts
│   │       │   ├── _utils/
│   │       │   │   └── priceFormatter.ts
│   │       │   └── _constants/
│   │       │       └── selectOptions.ts
│   │       └── _types/
│   │           └── index.ts
│   │
│   ├── components/
│   │   ├── ui/
│   │   │   ├── common/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Card.tsx
│   │   │   │   └── Modal.tsx
│   │   │   └── form/
│   │   │       └── Input.tsx
│   │   └── layout/
│   │       ├── Header.tsx
│   │       └── Footer.tsx
│   │
│   ├── lib/
│   │   ├── config/
│   │   │   ├── env.ts
│   │   │   └── serverEnv.ts
│   │   ├── security/
│   │   ├── hooks/
│   │   │   └── useDebounce.ts
│   │   ├── services/
│   │   │   ├── apiError.ts
│   │   │   ├── handleCommonErrorServer.ts
│   │   │   └── handleCommonErrorClient.ts
│   │   ├── utils/
│   │   │   └── formatDate.ts
│   │   └── doc/
│   │
│   ├── types/
│   │   └── index.ts
│   │
│   └── constants/
│       └── routes.ts
│
├── .env
├── .env.example
├── .gitignore
├── next.config.js
├── tsconfig.json
└── package.json
```

### 詳細

| ディレクトリ | 役割・説明 |
| --- | --- |
| **`/api`** | 外部APIサーバーとの連携に関するものを配置する層。配下に `openapi/`（自動生成物）を持つ。 |
| **`/app`** | 画面（Page）やWebブラウザからのリクエストを受け付け、レスポンス（HTML）を返却するコントローラー兼ルーティング層。 |
| **`/components`** | 複数のページをまたいで共通で利用する、ボタンや入力欄などのUIパーツ（見た目）・レイアウトを配置・管理する層。 |
| **`/lib`** | アプリケーション全体の共通ロジックや、日付操作・文字列加工など、画面から切り離された共通の裏方処理（関数群）を隠蔽する層。 |
| **`/types`** | アプリ内で共通利用するデータの構造やUI用のプロパティなど、プロジェクト全体で使い回す純粋な型定義（TypeScriptの型）を扱う層。 |
| **`/constants`** | 画面遷移のURLパスの定義や、システム全体で共有する固定の名称・設定値などを管理する定数配置層。 |
| **`/public`** | 画像、フォント、Faviconなど、ビルドせずにそのままブラウザから直接アクセスさせたい静的ファイルを配置する層。 |

## ディレクトリ詳細

### ・/api

**/api** ： 外部APIサーバー（Spring Boot）との連携に関するものを配置する層。

#### 詳細

| ディレクトリ / ファイル | 詳細 |
| --- | --- |
| **`openapi/`** | Spring Boot（springdoc-openapi）が出力するOpenAPIスキーマから、OrvalでFetch Clientとして一括生成した型定義・API通信関数を配置するディレクトリ。 |
| **`openapi/generated/index.ts`** | 配下の各タグ別ディレクトリ・共通モデルの内容を re-export する自動生成ファイル。利用側はこの `index.ts` 経由で参照できる。 |
| **`openapi/generated/model/`** | `output.schemas` で指定したスキーマ型の出力先。参照しているタグ数に関わらず、リクエスト/レスポンスのスキーマ型はすべてここにフラットに生成される（`{tag}/{tag}.ts` 側にはAPI通信関数のみが生成される）。 |
| **`openapi/generated/{tag}/{tag}.ts`** | Orvalが `@Tag`（Controller単位）ごとに分割生成する、APIのリクエスト/レスポンス型・URL生成関数（`getXxxUrl`）・`fetch` 実行までを含むAPI通信関数 |

#### 運用ルール

- 生成コマンド（例：`npm run generate:api`）を実行して生成する。**手動での作成・編集は禁止**。
- Orvalの出力設定は `client: 'fetch'` を使用し、**zodスキーマは生成しない**（設定は「付録：Orval設定」を参照）。
- Orvalの出力設定は `mode: 'tags-split'` を使用し、Spring側の `@Tag`（Controller単位）ごとにファイルを分割生成する。ファイル肥大化を防ぐため、1タグ1ファイルに収まる粒度で Spring側の `@Tag` を設計する。
- `output.schemas` に `model/` を指定し、スキーマ型はすべて `model/` 配下へ出力する。特定タグのフォルダ（`users/`など）へ手動で振り分けることは行わない。
- Spring側で「同じ意味のデータ（Userなど）は同一DTOクラスを使い回す」設計にしておくことで、同じ構造の型が別名で重複生成されるのを防ぐ。
- Spring側のController / DTOを変更した場合は、必ず再生成コマンドを実行し、差分をコミットする。
- ファイル冒頭の自動生成コメント（Orvalが自動付与）を削除しない。
- 生成されたAPI通信関数は戻り値として `{ data, status, headers }` を返し、**HTTPエラー時も例外はthrowされない**。ただしネットワーク断など `fetch` 自体が失敗した場合は例外がthrowされるため、`_services` ではステータス判定と例外捕捉の両方を行う。
- ステータス判定とエラー制御は必ず `_services` 側で行う。うち全画面共通のエラー（認証切れなど）は `src/lib/services/` の共通ハンドラを経由する。
- APIレスポンスのランタイム検証はフロントエンドでは行わない。OpenAPIスキーマとバックエンド実装の齟齬はバックエンド側のテストで担保する。
- `openapi/generated/` 配下のファイルを **他ディレクトリへ手動コピーすることは禁止**。利用する場合は必ず `import` する。
- `openapi/generated/` 配下の**API通信関数**（`getUser` など）を `_services` 以外のファイル（`page.tsx`, `_components`, `_hooks` など）から**直接importすることは禁止**する。必ず `_services` を経由する。
- `openapi/generated/` 配下の**型**を `_types`,`/types/` 以外のファイル（`page.tsx`, `_components`, `_hooks` など）から**直接importすることは禁止**する。必ず後述の `_types`,`/types/` を経由する。
- import は分割後の個別ファイルではなく、必ず `openapi/generated`（`index.ts`）から行う。

---

### ・/app

**/app** ： 画面（Page）やWebブラウザからのリクエストを受け付け、レスポンス（HTML）を返却するコントローラー兼ルーティング層。
AppRouterを使用しAPI通信、画面の状態管理、バリデーションチェックなどを行う

#### 詳細

`/app`にはページごと(ページを表すディレクトリごと)に以下のファイル・ディレクトリを配置する

**/app 配下のディレクトリ構成（ページ単位）**

| ファイル / ディレクトリ | 詳細 |
| --- | --- |
| **`page.tsx`** | ページの画面（View）を表示するエントリーポイント |
| **`layout.tsx`** | そのページとその配下のページで共通利用するレイアウト（オプション） |
| **`_components/`** | そのページ内だけで使う専用のUIコンポーネント群 |
| **`_types/`** | そのページ内だけで使う専用の型定義（`index.ts` など） |
| **`_lib/`** | そのページ内だけで使う専用のロジックを管理するフォルダ群 |

---

- *_lib/ 配下のサブディレクトリ**

| ディレクトリ | 詳細 |
| --- | --- |
| **`_services/`** | 外部APIサーバーとの通信（Orval生成関数の呼び出しとエラー制御） |
| **`_hooks/`** | 画面の状態管理やライフサイクルを扱うカスタムフック |
| **`_schemas/`** | 入力バリデーションやデータ検証のルール（Zodなど） |
| **`_utils/`** | そのページ内専用の共通処理や計算を行う関数群 |
| **`_constants/`** | そのページ内専用の選択肢や固定値などの定数群 |

#### type の置き場所

| 種類 | 該当する型 | 配置場所 |
| --- | --- | --- |
| API自動生成の型 | Spring由来のリクエスト/レスポンス型・API通信関数 | `src/api/openapi/generated/`（自動生成・編集禁止） |
| 共通に置くもの | DB / API に紐づくデータモデル（User, Article, Product など）、アプリ全体で使い回す型。`openapi/generated`（主に`model/`）の型を**複数ページ・複数コンポーネントで使い回す場合**はここで受けて再定義する | `src/types/`（ルート直下） |
| _types に置くもの | ・fetch（_services）の戻り値の型など、そのページ内で複数ファイル（_services → _hooks → _components / page.tsx）をまたいで使う型、`page.tsx` で扱う`PageProps` を除く型定義。`openapi/generated`の型を**そのページ内でしか使わない場合**はここで受けて再定義する | `src/app/xxx/_types/` |
| 各ファイルに書くもの（インライン） | コンポーネントの Props、PageProps、useState の型、useForm の入力値の型など、そのファイル単体で完結する型。 | 各コンポーネント / page.tsx の上部 |

#### hooks の置き場所

| 種類 | 該当するhooks | 配置場所 |
| --- | --- | --- |
| 共通に置くもの | 複数ページで使い回す汎用hooks（useDebounce, useLocalStorage など、特定のドメインに依存しないもの） | `src/lib/hooks/` |
| _hooks に置くもの | ・そのページ専用のデータ取得・フォーム・状態管理hooks（useDashboardData, useSampleData など）。 | `src/app/xxx/_lib/_hooks/` |
| 各ファイルに書くもの（インライン） | そのコンポーネント特有のhooks(useState 自体、単純な useEffect・UI描画関係 など、hooksとして切り出すほどでもない状態管理) | 各コンポーネントファイル内に直接記述（フック化しない） |

**ディレクトリ別詳細**

- **page.tsx**
    - 画面（UI）の描画に専念し、API通信や複雑なデータ加工などのロジックは直接書かずに `_lib` へ任せる。
    - Server Component（サーバー側での描画）として実装し、状態管理・イベント・Client Components(クライアント側で描画)やそのページのみのUIパーツを `_components` に切り出す(`use client`を記載してはならない）。
    - URLから渡されるパラメータ（params / searchParams）を受け取る最初の窓口とし、配下へ渡す役割を担う。
    - サーバーサイドでのデータ取得（フェッチ）処理は `_components` 内で直接サービスを呼び出さず、必ず `page.tsx` から `_services` を呼び出して行う。
    - type周りの処理は`PageProps`はインライン(上部に分離して記述する）に直接書き、それ以外は書かない(`_types/`に分離する)。
    - hooks周りの処理は書かない
- **layout.tsx**
    - 画面遷移時に再レンダリングさせたくない、そのページ配下で共通して使うヘッダーやサイドバーなどのレイアウトを定義する。
    - ページの本体コンテンツを表示するために、必ず引数で `children` を受け取って枠組みの中に配置する。
- *_components**
    - そのページの中だけで使い、他のページでは絶対に使い回さない専用のUIコンポーネントのみを配置する。
    - `page.tsx` が長くなるのを防ぐため、フォームやカードといったパーツ単位でファイルを適切に細分化する。
    - 状態管理やClient Components・Server Componentsを配置する
    - Client Components(`use client`)を使う記述は必ず`_components/`に書き出す(`page.tsx`には記載しない)。
    - 1ファイル1コンポーネントで実装する
- *_types**
    - フォームの入力項目や、そのページ固有のUI表示用データなど、このページ専用の型定義のみを管理する。
    - システム全体で共通して使うモデルなどの型は、ここではなくルート直下の `/types` から読み込んで利用する。
    - `src/api/openapi/generated/` の型を利用する場合は、他ファイルから直接importさせず、必ずここで受けて再定義（re-export、または加工）してから配下へ渡す。
    - `PageProps` のみは`_types/` に記述せず、`page.tsxに記述する`
- *_services**
    - APIサーバーに対して、データの取得（GET）や送信（POST/PUT等）を行う通信処理のみを定義する。
    - **通信処理そのものは自前で `fetch` を書かず、`openapi/generated` が生成したAPI通信関数を呼び出す**。`_services` はその薄いラッパーとして、ステータス判定・エラー制御・呼び出し側に返す形への整形のみを担う。
    - Orval生成関数はHTTPエラー時に例外をthrowしないため、**`_services` で必ずステータスを判定し、異常時はthrowする**。throwするエラーは `src/lib/services/` の `ApiError` に統一する。
    - **エラー処理の責務分担**
        - 全画面で対応が同じエラー（認証切れ・権限なし・サーバ障害・ネットワーク断）は自前で書かず、`src/lib/services/` の共通ハンドラを呼び出して処理させる。
        - それ以外の、画面ごとに文言や挙動が変わるエラー（404の未存在、400のバリデーション、409の競合、業務エラーなど）は各 `_services` 内で個別に判定してthrowする。
        - 実装順序は「共通ハンドラを先に通し、その後で個別判定」とする。
    - 共通ハンドラは内部で `redirect()` を使う場合があり、これは例外機構で実現されている。共通ハンドラの呼び出しを `try/catch` で囲むと遷移が中断されるため囲まないこと。
    - **サーバーサイドとクライアントサイドでエラーの伝わり方が異なる**ため、以下の制約に従う。
        - Server Component から外へ抜けた例外は、Next.jsが `error.tsx`（Client Component）へ渡す境界でオブジェクトを作り直す。production ビルドでは `message` が汎用文字列へ差し替えられ、`status` や `fieldErrors` といった独自プロパティはすべて剥がされる（情報漏洩防止のための仕様）。**`error.tsx` 側で `error.status` を読むことはできない。**
        - この差し替えは `next dev` では行われないため、開発中は動いているように見える。必ず `next build && next start` で確認する。
        - したがって、**サーバーサイドの `_services` ではステータスに応じた画面の作り分けを行わない**。データが存在しない場合（404）は `ApiError` をthrowせず Next.js の `notFound()` を呼び、`not-found.tsx` に処理を任せる。それ以外の異常は `ApiError` をthrowし、`error.tsx` の汎用エラー画面で受ける。
        - `error.tsx` でどうしてもステータス単位の分岐が必要な場合は、越境しても保持される `digest` フィールドのみを利用する。`digest` は文字列前提のため、`fieldErrors` のような構造化データは運べない。
        - **ステータスに応じた詳細な分岐（400のフィールドエラー表示、409の競合メッセージなど）はクライアントサイドでのみ行う。** クライアント側は同一ランタイム内で例外が伝わるため、`ApiError` のプロパティはそのまま `_hooks` の catch で参照できる。
    - 通信と最低限のエラー制御のみを行い、画面にまつわる状態（State）はここでは一切扱わない。
    - サーバサイドでのみAPIサーバーからデータを取得するファイルには必ず`'server-only'` を記述する
    - サーバサイドで実行するファイルは○○Server.ts、クライアントサイドで実行するファイルは○○Client.tsとする
    - 1ファイル1公開関数(private関数は除く)で実装する
    
    **エラー処理の責務対応表**
    
    | ステータス / 事象 | 処理場所 | サーバーサイドでの挙動 | クライアントサイドでの挙動 |
    | --- | --- | --- | --- |
    | 401 認証切れ | `lib/services/`（共通） | `redirect()` でログイン画面へ | `window.location` でログイン画面へ |
    | 403 権限なし | `lib/services/`（共通） | `ApiError` をthrow → `error.tsx` の汎用表示 | `ApiError` をthrow → `_hooks` でトースト表示等 |
    | 5xx サーバ障害 | `lib/services/`（共通） | `ApiError` をthrow → `error.tsx` の汎用表示 | `ApiError` をthrow → `_hooks` でトースト表示等 |
    | ネットワーク断 | `lib/services/`（共通） | `ApiError(status: 0)` をthrow | `ApiError(status: 0)` をthrow |
    | 404 未存在 | 各 `_services` | **`notFound()`** を呼び `not-found.tsx` に任せる | `ApiError` をthrow → `_hooks` で表示 |
    | 400 バリデーション | 各 `_services` | 発生しない（サーバー側は参照系のみ） | `fieldErrors` を詰めてthrow → `_hooks` で `setError` |
    | 409 競合・業務エラー | 各 `_services` | `ApiError` をthrow → `error.tsx` の汎用表示 | `ApiError` をthrow → `_hooks` で文言表示 |
    
    ※ サーバーサイドでは `status` が `error.tsx` まで届かないため、404 以外はすべて汎用エラー画面となる。ステータスごとの文言の出し分けが要件にある画面は、クライアントサイドで取得する設計にする。
    
- *_hooks**
    - ページ内カスタムフックス(useState・useEffect・useForm等)などの定義行う
    - ボタンを押したときのアクションや読み込み中（Loading）の判定など、コンポーネントから「動作の仕組み」を引き剥がして一括管理する。
    - APIへのデータ送信を伴うフォームは、`useForm` をこの `_hooks/` 内で定義する
    - サーバー側のバリデーションエラー（400）は `_services` からthrowされた `ApiError` をここで受け、その `fieldErrors` を `setError` などでフォームに反映する。認証切れなどの共通エラーは `lib/services/` 側で処理済みのため、ここでは扱わない。
    - 1ファイル1公開関数(private関数は除く)で実装する
- *_schemas**
    - Zodなどを使った入力チェック（バリデーション）の定義を置く。
    - 定義したスキーマから型を抽出して、フォーム送信時のバリデーションや表示の検証ルールとして使用する。
    - **フォームのスキーマは、APIのスキーマとは別物として、このディレクトリ内で自前定義する**。`src/api/openapi/generated/` から派生させたり、そこからimportしたりしてはならない。
    - 仮に現時点でAPIのスキーマと内容が一致していても、必ず別で定義する（郵便番号の全角許容、数値のカンマ区切り表示、入力途中の段階的な形状など、フォーム側とAPI側で扱う値の形は一致しない場合があるため）。
    - フロントエンドで行うバリデーションは**UXのため**のものと位置づける。セキュリティや不正値の防止を目的としたバリデーションはバックエンド（Spring側の `@Valid` 等）の責務であり、フロントエンドで同じ検証を厳密に二重実装することは求めない。
    - フォームの値をAPIへ送信する際は、フォーム用の型からAPIのリクエスト型への変換（全角→半角変換、文字列→数値変換、確認用フィールドの除去など）を行ってから `_services` に渡す。
- *_utils**
    - 日付の整形や金額のカンマ区切りなど、特定の「状態」を持たずに、入力に対して決まった値を返す純粋な加工関数のみを置く。
    - アプリ全体で使い回す便利な関数はルートの `/lib/utils` に任せ、ここではそのページ専用の加工処理のみに限定する。
- *_constants**
    - セレクトボックスの選択肢（都道府県など）や、そのページ専用の初期表示設定といった、途中で変わることのない固定の定義値を置く。
    - アプリ全体で共有する共通の定数やシステム設定は、ルート直下の `/constants` を使用する。

#### **制約**

- page.tsxには描画などの最低限の処理のみ記述し、API通信・状態管理など他の処理はサブディレクトリ内に別ファイルに記述する
- 他の画面などで使う処理は共通の(`src/`直下の)`lib/`・`components/`・`types/`・`constants/`などに配置する
- バリデーションチェックはzodライブラリを利用する（スキーマはAPIのスキーマから生成せず `_schemas` で自前定義する）
- サブディレクトリにはディレクトリ名の前に`_`をつける
- `src/api/openapi/generated/` 配下のファイルは自動生成物であり、手動編集・手動コピーを禁止する。利用する場合は、API通信関数は必ず `_services` を経由し、型は必ず `_types` / `/types/` を経由してimportする
- 全画面共通のエラー処理は `src/lib/services/` に集約し、画面固有のエラー処理は各 `_services` で行う
- APIサーバ通信の流れ
    - サーバーサイド
    page.tsx (Server Component) ───> _services ───> openapi/generated (Orval生成関数)
    - クライアントサイドで動的読み込み
    _components (Client UI) ──> _hooks ────> _services ───> openapi/generated (Orval生成関数)
    - 共通エラー処理
    _services ───> lib/services (共通ハンドラ)
- サーバサイドコンポーネントでは`useEffect` は使用せず直接非同期でフェッチする

---

### /components

複数のページをまたいで共通で利用する、ボタンや入力欄などのUIパーツ（見た目）・レイアウトを配置・管理する層。

- `layout/`、`form`、`ui` など役割ごとにディレクトリ分けを行う。
- Props の型定義は、そのコンポーネント特有のものはインラインで記述する（`types/` 相当の分離ディレクトリは持たない）。
- カスタムhooksを持つ場合は UI の表示まわり（開閉、ホバー、アニメーションなど）のみに限定し、
データ取得・ドメインに依存する状態管理は行わない（親から props で受け取る）。
- 値のやり取りは基本的に props で行う（状態を持つ場合は該当コンポーネント内に閉じる）。
- Client Components の利用は必要最小限の範囲で使用し、`use client` が不要な表示専用コンポーネントは
Server Component のままにする。
- 1ファイル1コンポーネントで実装する

---

### /lib

アプリケーション全体の共通ロジックや、日付操作・文字列加工など、画面から切り離された共通の裏方処理（関数群）を隠蔽する層。

- **config/**
    - `.env` から読み込んだ値をそのまま各所で使わず、ここで型付け・検証してから提供する。
- **security/**
    - APIサーバーへのリクエストに必要な認証トークンの取得・付与、
    およびログイン状態に基づく画面アクセスの制御を行う。
    - トークンの検証や権限判定そのものはAPIサーバー側の責務であり、ここでは扱わない。
- **hooks/**
    - `useDebounce` のように「値を受け取って加工した値を返す」など、ドメインに依存しない汎用hooksのみを置く。
    - ページやドメイン固有の状態（フィルタ条件、フォームの入力値など）はここに置かず `_hooks/` へ。
    - 1ファイル1公開関数(private関数は除く)で実装する
- **services/**
    - fetchの共通設定（baseURL、共通ヘッダー、タイムアウト）や、複数ページで使い回すAPI通信処理を置く。
    - **全画面で同一の挙動をとるエラーの処理をここで一元的に定義する**。
    具体的には認証切れ（401）、権限なし（403）、サーバ障害（5xx）、ネットワーク断など、
    どの画面から呼ばれても対応が変わらないものを対象とする。
    - アプリ共通のエラー型 `ApiError`（`status` / `message` / `fieldErrors` を保持）を定義し、
    `_services` からthrowするエラーはすべてこの型に統一する。
    これにより `_hooks` や `error.tsx` 側でステータスに応じた分岐が可能になる。
    - 共通ハンドラは、対象ステータスであれば redirect または throw して処理を打ち切り、
    対象外であれば何もせず返す。個別のステータス判定は呼び出し側の `_services` に委ねる。
    - サーバー用は `○○Server.ts`、クライアント用は `○○Client.ts` と命名する（`_services` と同じ命名規則）。
    認証切れ時の遷移方法がサーバー（`redirect`）とクライアント（画面リロードを伴う遷移）で異なるため、
    共通ハンドラも両者を分けて実装する。
    - 1ファイル1公開関数(private関数は除く)で実装する。ただしエラー型（クラス）の定義ファイルはこの限りではない。
- **utils/**
    - `formatDate` のように、入力に対して決まった値を返すだけの純粋関数のみを置く。
    - ページ固有の加工処理は置かず `_utils/` へ。
- **doc/**
    - 実装方針や設計判断など、コードに残しづらい情報をドキュメントとして残す。

---

### /types

アプリ内で共通利用するデータの構造（DB / API に紐づくモデルなど）を管理する層。

- DB や API のレスポンスに紐づく、複数ページ・複数コンポーネントで使い回すデータモデル型（`User`, `Article`, `Product` など）のみを置く。
- `src/api/openapi/generated/`（主に`model/`）の型を複数ページで共通利用する場合は、ここで受けて再定義（re-export）してから使う。
- ページ固有の型（フォームの入力値、UI表示専用データなど）は置かず `_types/` へ。
- ドメインごとにファイルを分割し `index.ts` で re-export する。

#### 判断基準

- `openapi/generated` 由来の型を `/types` に置くか `_types` に置くかの判断基準：
    - **アプリ全体（2つ以上のページ・コンポーネント）で使い回す場合** → `src/types/` に置く
    - **特定の1ページ内でしか使わない場合** → そのページの `_types/` に置く
    - 迷った場合は、まず `_types/` に置き、2箇所目で利用が発生した時点で `src/types/` へ移動する

---

### /constants

画面遷移のURLパスの定義や、システム全体で共有する固定の名称・設定値などを管理する定数配置層。

- 複数ページで共有するルーティングパス（`routes.ts`）、システム全体の固定値（ステータスコード、権限区分など）のみを置く。
- ページ固有の選択肢や初期値（セレクトボックスの選択肢など）は置かず `_constants/` へ。

---

## 付録：Orval設定

```tsx
// orval.config.ts
import { defineConfig } from 'orval';

export default defineConfig({
  sample: {
    input: { target: './openapi.json' },
    output: {
      mode: 'tags-split',
      client: 'fetch',
      target: 'src/api/openapi/generated',
      // 未指定だと model/ が生成されず target 側にスキーマが混在するため必須
      schemas: 'src/api/openapi/generated/model',
      // index.ts 経由の import を前提とするため明示（デフォルトtrue）
      indexFiles: true,
      // 削除されたエンドポイントのファイルが残らないよう生成前にクリーンする
      clean: true,
    },
  },
});
```

## 付録：サンプルコード

### 生成されるコードのイメージ

```tsx
// src/api/openapi/generated/users/users.ts（自動生成・編集禁止）
export type getUserResponse200 = { data: UserResponse; status: 200 };
export type getUserResponse404 = { data: null; status: 404 };
export type getUserResponse = (getUserResponse200 | getUserResponse404) & {
  headers: Headers;
};

export const getGetUserUrl = (userId: number) => `/api/v1/users/${userId}`;

export const getUser = async (
  userId: number,
  options?: RequestInit,
): Promise<getUserResponse> => { /* fetch実行 */ };
```

### lib/services（共通エラー型と共通ハンドラ）

```tsx
// src/lib/services/apiError.ts
export class ApiError extends Error {
  constructor(
    readonly status: number, // 0 はネットワークエラーを表す
    message: string,
    readonly fieldErrors?: Record<string, string>,
  ) {
    super(message);
    this.name = 'ApiError';
  }
}
```

```tsx
// src/lib/services/handleCommonErrorServer.ts
import 'server-only';
import { redirect } from 'next/navigation';
import { ROUTES } from '@/constants/routes';
import { ApiError } from './apiError';

/**
 * 全画面共通のエラーを処理する。
 * 対象ステータスの場合は redirect または throw するため後続処理には進まない。
 * 対象外の場合は何もせず返るので、個別判定は呼び出し側の _services で行う。
 */
export const handleCommonErrorServer = (status: number): void => {
  if (status === 401) {
    redirect(ROUTES.LOGIN); // 内部で例外をthrowするため try/catch で囲まないこと
  }
  if (status === 403) {
    throw new ApiError(403, 'この操作を行う権限がありません');
  }
  if (status >= 500) {
    throw new ApiError(status, 'サーバーで問題が発生しました');
  }
};
```

```tsx
// src/lib/services/handleCommonErrorClient.ts
import { ROUTES } from '@/constants/routes';
import { ApiError } from './apiError';

export const handleCommonErrorClient = (status: number): void => {
  if (status === 401) {
    // 保持している状態を破棄するため、ルーター遷移ではなく画面遷移させる
    window.location.href = ROUTES.LOGIN;
    throw new ApiError(401, '認証の有効期限が切れました');
  }
  if (status === 403) {
    throw new ApiError(403, 'この操作を行う権限がありません');
  }
  if (status >= 500) {
    throw new ApiError(status, 'サーバーで問題が発生しました');
  }
};
```

### _services（Orval生成関数の呼び出し）

```tsx
// _lib/_services/getSampleItemsServer.ts
import 'server-only';
import { getSampleItems } from '@/api/openapi/generated';
import { ApiError } from '@/lib/services/apiError';
import { handleCommonErrorServer } from '@/lib/services/handleCommonErrorServer';
import type { SampleItem } from '../../_types';

export const fetchSampleItems = async (): Promise<SampleItem[]> => {
  // fetch 自体の失敗（ネットワーク断など）は例外でthrowされるため捕捉する
  const res = await getSampleItems().catch(() => {
    throw new ApiError(0, '通信に失敗しました。時間をおいて再度お試しください');
  });

  // 認証切れ・権限・サーバ障害など、全画面共通のエラーを先に処理させる
  handleCommonErrorServer(res.status);

  // ここから下はこの画面固有のエラー判定
  if (res.status !== 200) {
    throw new ApiError(res.status, '一覧の取得に失敗しました');
  }
  return res.data;
};
```

### _services（サーバーサイド：404 は notFound() を使う）

```tsx
// _lib/_services/getSampleItemServer.ts
import 'server-only';
import { notFound } from 'next/navigation';
import { getSampleItem } from '@/api/openapi/generated';
import { ApiError } from '@/lib/services/apiError';
import { handleCommonErrorServer } from '@/lib/services/handleCommonErrorServer';
import type { SampleItem } from '../../_types';

export const fetchSampleItem = async (id: number): Promise<SampleItem> => {
  const res = await getSampleItem(id).catch(() => {
    throw new ApiError(0, '通信に失敗しました。時間をおいて再度お試しください');
  });

  handleCommonErrorServer(res.status);

  // 404 は ApiError をthrowせず notFound() を呼ぶ。
  // ApiError の status は error.tsx まで届かないため、Next.js の仕組みに任せる。
  if (res.status === 404) {
    notFound(); // 内部で例外をthrowするため try/catch で囲まないこと
  }

  if (res.status !== 200) {
    // ここに来た場合 error.tsx では汎用エラー画面が表示される（status は届かない）
    throw new ApiError(res.status, '情報の取得に失敗しました');
  }
  return res.data;
};
```

### _services（クライアントサイド：status に応じた分岐が可能）

```tsx
// _lib/_services/updateSampleItemClient.ts
import { updateSampleItem } from '@/api/openapi/generated';
import { ApiError } from '@/lib/services/apiError';
import { handleCommonErrorClient } from '@/lib/services/handleCommonErrorClient';
import type { SampleItem, SampleItemRequest } from '../../_types';

export const putSampleItem = async (
  id: number,
  body: SampleItemRequest,
): Promise<SampleItem> => {
  const res = await updateSampleItem(id, body).catch(() => {
    throw new ApiError(0, '通信に失敗しました。時間をおいて再度お試しください');
  });

  handleCommonErrorClient(res.status);

  // クライアントサイドは同一ランタイム内で例外が伝わるため、
  // status / fieldErrors がそのまま _hooks の catch で参照できる
  if (res.status === 400) {
    throw new ApiError(400, '入力内容に誤りがあります', res.data.fieldErrors);
  }
  if (res.status === 404) {
    throw new ApiError(404, '対象のデータが見つかりませんでした');
  }
  if (res.status === 409) {
    throw new ApiError(409, '他のユーザーによって更新されています。再読み込みしてください');
  }
  if (res.status !== 200) {
    throw new ApiError(res.status, '更新に失敗しました');
  }
  return res.data;
};
```

### _hooks（クライアント側での status 分岐）

```tsx
// _lib/_hooks/useSampleForm.ts
'use client';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { ApiError } from '@/lib/services/apiError';
import { putSampleItem } from '../_services/updateSampleItemClient';
import { sampleFormSchema, type SampleFormValues } from '../_schemas/formSchema';

export const useSampleForm = (id: number) => {
  const form = useForm<SampleFormValues>({
    resolver: zodResolver(sampleFormSchema),
  });

  const onSubmit = form.handleSubmit(async (values) => {
    try {
      await putSampleItem(id, toRequest(values));
    } catch (e) {
      if (!(e instanceof ApiError)) throw e;

      // 401 は共通ハンドラが画面遷移を開始済み。遷移中に不要な表示を出さない
      if (e.status === 401) return;

      // 400 はフィールド単位でフォームへ反映する
      if (e.status === 400 && e.fieldErrors) {
        Object.entries(e.fieldErrors).forEach(([name, message]) => {
          form.setError(name as keyof SampleFormValues, { message });
        });
        return;
      }

      // それ以外はフォーム全体のエラーとして表示する
      form.setError('root', { message: e.message });
    }
  });

  return { form, onSubmit };
};
```

### error.tsx / not-found.tsx（サーバーサイドの受け皿）

```tsx
// app/sample/[id]/not-found.tsx
// サーバーサイドの _services が notFound() を呼んだ場合に表示される
export default function NotFound() {
  return <p>お探しのデータは見つかりませんでした。</p>;
}
```

```tsx
// app/sample/[id]/error.tsx
'use client';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  // 注意：production では error.message は汎用文字列に差し替えられ、
  // ApiError の status / fieldErrors は剥がされているため参照できない。
  // ステータス単位の分岐が必要な場合は digest のみが利用可能。
  return (
    <div>
      <p>問題が発生しました。時間をおいて再度お試しください。</p>
      {error.digest && <p>エラーID: {error.digest}</p>}
      <button onClick={reset}>再読み込み</button>
    </div>
  );
}
```

### _schemas（フォーム用スキーマの自前定義）

```tsx
// _lib/_schemas/formSchema.ts
import { z } from 'zod';

// APIのスキーマとは独立してフォーム用に定義する
export const signupFormSchema = z
  .object({
    email: z.string().email('メールアドレスの形式が正しくありません'),
    password: z.string().min(8, 'パスワードは8文字以上で入力してください'),
    confirmPassword: z.string(),
    // UXのため全角も受け付け、送信時に半角へ変換する
    postalCode: z
      .string()
      .regex(/^[0-9０-９]{3}-?[0-9０-９]{4}$/, '郵便番号の形式が正しくありません'),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: 'パスワードが一致しません',
    path: ['confirmPassword'],
  });

export type SignupFormValues = z.infer<typeof signupFormSchema>;
```

---

# 命名規則

| 対象要素 | 推奨される表記法 (ケース) | 具体的な命名例 |
| --- | --- | --- |
| **コンポーネントファイル** | アッパーキャメルケース + `.tsx` | `SampleForm.tsx`, `DataCard.tsx`, `Button.tsx` |
| **コンポーネント名** | アッパーキャメルケース | `SampleForm`, `DataCard` |
| **その他のファイル** | ローワーキャメルケース + `.ts` | `priceFormatter.ts`, `formatDate.ts`, `apiError.ts` |
| **App Router 予約ファイル** | すべて小文字（Next.js規定） | `page.tsx`, `layout.tsx`, `error.tsx`, `not-found.tsx`, `loading.tsx` |
| **ルーティング用ディレクトリ** | ケバブケース（URLに露出するため） | `user-profile/`, `order-history/` |
| **プライベートディレクトリ** | `_` + ローワーキャメルケース | `_components/`, `_lib/`, `_services/` |
| **関数 / 変数** | ローワーキャメルケース | `fetchSampleItems`, `userId`, `totalPrice` |
| **定数** | 大文字スネークケース | `MAX_RETRY_COUNT`, `DEFAULT_PAGE_SIZE` |
| **定数オブジェクト** | 大文字スネークケース（キーも大文字） | `ROUTES.LOGIN`, `STATUS.ACTIVE` |
| **型 / インターフェース** | アッパーキャメルケース | `SampleItem`, `User`, `ApiError` |
| **Props の型** | コンポーネント名 + `Props` | `SampleFormProps`, `DataCardProps` |
| **カスタムフック** | `use` + アッパーキャメルケース | `useSampleData`, `useDebounce` |
| **クラス** | アッパーキャメルケース | `ApiError` |
| **boolean 変数** | `is` / `has` / `can` / `should` を接頭辞にする | `isLoading`, `hasError`, `canSubmit` |
| **イベントハンドラ（実装側）** | `handle` + 対象 + 動作 | `handleSubmit`, `handleNameChange` |
| **イベントハンドラ（Props側）** | `on` + 対象 + 動作 | `onSubmit`, `onNameChange` |
| **zodスキーマ** | ローワーキャメルケース + `Schema` | `signupFormSchema`, `searchFormSchema` |
| **スキーマ由来の型** | アッパーキャメルケース + `Values` | `SignupFormValues` |
| **型パラメータ (Generics)** | 大文字1文字 | `T`, `E`, `K`, `V` |
| **環境変数** | 大文字スネークケース | `API_BASE_URL`, `NEXT_PUBLIC_APP_ENV` |

## `_services` の関数名・ファイル名

| 対象 | ルール | 例 |
| --- | --- | --- |
| ファイル名 | 動詞 + 対象 + 実行環境（`Server` / `Client`） | `getSampleItemsServer.ts`, `updateSampleItemClient.ts` |
| 関数名 | HTTPメソッドを想起させる動詞 + 対象 | `fetchSampleItems`, `putSampleItem`, `postSampleItem` |
- ファイル名と関数名は一致させなくてよい。ファイル名は「どこで実行されるか」、関数名は「何をするか」を表す。
- 1ファイル1公開関数のため、ファイル名から公開関数が一意に特定できる粒度を保つ。

---

# コーディング規約

## 書式

| 分類 | ルール設定内容 | 具体例・推奨値 |
| --- | --- | --- |
| **インデント (Indent)** | スペース2個（タブ文字は禁止） | Prettier のデフォルトに従う |
| **改行コード (Line End)** | `LF` に統一 | Windows環境でも `CRLF` ではなく `LF`（`.gitattributes` で強制する） |
| **1行の文字数制限** | 最大150文字（超える場合は改行） | Prettier の `printWidth: 100` |
| **セミコロン** | 文末のセミコロンは必須 | `const a = 1;` |
| **クォート** | シングルクォートを使用（JSXの属性値はダブルクォート） | `import { z } from 'zod';` / `<div className="flex">` |
| **末尾カンマ (Trailing Comma)** | 複数行に分かれる場合は必ず付ける | Prettier の `trailingComma: 'all'` |
| **波括弧の位置 (Braces)** | 行の末尾に開始括弧 `{` を置く（K&Rスタイル） | `if (condition) {` → 改行 → 処理 → `}` |
| **余分な空白 (Whitespace)** | キーワードの前後、演算子の前後にはスペースを1つ空ける | `if (a === b)`（`if(a===b)` はNG） |
| **ファイルの末尾** | ファイルの最後には必ず1つの改行を入れる | 末尾に空行を1行 |
| **書式の担保** | 手動で整えず Prettier に任せる。CIで `prettier --check` を実行する | 書式についてのレビュー指摘は行わない |

---

# 使用ライブラリ

## 確定しているもの

| 分類 | ライブラリ | 用途 |
| --- | --- | --- |
| フレームワーク | Next.js（App Router） | ルーティング・SSR・ストリーミング |
| UIライブラリ | React | コンポーネント描画 |
| 言語 | TypeScript | 静的型付け |
| APIクライアント生成 | Orval | OpenAPIスキーマから型定義・fetch関数を生成 |
| バリデーション | zod | フォーム入力検証・環境変数検証 |
| フォーム | react-hook-form | フォームの状態管理・送信制御 |
| フォーム連携 | @hookform/resolvers | zodスキーマを react-hook-form に接続 |
| サーバー限定化 | server-only | サーバー専用モジュールのクライアント混入を防止 |
| Lint | ESLint / eslint-config-next | 静的解析・import制約の強制 |
| Formatter | Prettier | 書式の自動整形 |

## 選定が必要なもの

以下は本規約では未確定。プロジェクト開始時に決定し、本項を更新する。

| 分類 | 候補 | 判断のポイント |
| --- | --- | --- |
| スタイリング | Tailwind CSS / CSS Modules | `globals.css` のみで運用するか、ユーティリティクラスを導入するか |
| UIコンポーネント | 自前実装 / shadcn/ui など | `components/ui/` を自前で作る前提なら不要 |
| テスト | Vitest / Jest + React Testing Library | テストコードの配置ルールとあわせて決定する |
| E2Eテスト | Playwright | 導入する場合は `e2e/` をリポジトリルートに置く |
| トースト通知 | react-hot-toast / sonner など | クライアント側のエラー表示方法とあわせて決定する |

## 方針

- **クライアントサイドのデータ取得ライブラリ（TanStack Query, SWR など）は導入しない。** データ取得は `_services` + `_hooks` の自前実装とし、Orval の出力も `client: 'fetch'` に固定する。
- 状態管理ライブラリ（Redux, Zustand など）は導入しない。状態はページ単位の `_hooks` に閉じる。
- 導入するライブラリを追加する場合は、本項に用途を明記してから追加する。

---

# 環境変数

## 一覧

| 変数名 | 用途 | 公開範囲 | 例 |
| --- | --- | --- | --- |
| `API_BASE_URL` | サーバーサイドから呼び出すAPIサーバーのベースURL | サーバーのみ | `http://api-server:8080` |
| `NEXT_PUBLIC_API_BASE_URL` | クライアントサイドから呼び出すAPIサーバーのベースURL | **ブラウザに露出** | `https://api.example.com` |
| `NEXT_PUBLIC_APP_ENV` | 動作環境の識別（表示切替・ログ制御用） | **ブラウザに露出** | `local` / `staging` / `production` |

- 認証方式（Cookie / Authorization ヘッダ）が確定した時点で、必要な変数を本表に追記する。
- `NODE_ENV` は Next.js が自動設定するため `.env` には記述しない。


## Gitのブランチ運用

#### ブランチ命名規則

種別/issue番号_タスク名_ユーザー名

| ブランチ種別 | 命名形式 | 例 | 用途 |
| --- | --- | --- | --- |
| main | main | main | 本番デプロイ対象。直接pushは禁止 |
| develop | develop | develop | 開発統合ブランチ。各featureはここにマージ |
| 機能開発 | feature/{issue番号}_{タスク名}_*{ユーザー名}* | feature/11_user-login_tanaka | 新機能・タスク単位の開発 |
| バグ修正 | fix/{issue番号}_{タスク名}_*{ユーザー名}* | fix/22_fix-login-error_tanaka | develop上で見つかったバグ修正 |
| 緊急修正 | hotfix/{issue番号}_{タスク名}*{ユーザー名}* | hotfix/33_fix-critical-bug_tanaka | 本番(main)で発生した緊急バグの修正 |
| インフラ整備 | infra/{issue番号}_{タスク名}*{ユーザー名}*	 | infra/33_docker-setup_tanaka | 環境構築・インフラ等 |

#### メッセージ規約

- 日本語で行う
- PRは変更内容まで記載する
- PRは親issueにサブissueして作成する

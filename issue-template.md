
# Issueテンプレート

GitHubのIssueを「親Issue」と「子Issue」で管理するためのテンプレート集です。
各テンプレートは「表示イメージ」の下に、コピー用のMarkdownを載せています。
「AI向け補足」は折りたたんでいます。AIにIssueを作らせるときは、開いてコピーして渡してください。

- 親Issue：複数の子Issueをまとめる大分類（Issue名は機能名のみ。例：認証機能、SQL管理）
- 子Issue：親Issueにぶら下げる個別タスク（Issue名は具体的な内容。例：認証機能の作成）
- すべての項目を書く必要はありません。不要な項目は省略してください。
- ラベル：親Issueには `親issue` を付けます。

## 目次

- [使い方](#usage)
- [親Issue（汎用）](#parent-generic)
- [子Issue（汎用）](#child-generic)
- [親Issue（API・CRUD）](#parent-api)
- [子Issue（API・CRUD）](#child-api)
- [親Issue（front・画面）](#parent-front)
- [子Issue（front・画面）](#child-front)
- [API・CRUD 作成手順](#child-creation-steps)

## 開発規約

- フロント：<https://mbm-projects.github.io/document/front-document>
- バック：<https://mbm-projects.github.io/document/back-document>

---

## 使い方 {#usage}

1. 親Issueを作成する（タイトルは機能名のみ、ラベル `親issue`）
2. 子Issueを作成し、「親Issue」に親のIssue番号（`#12` など）を書く
3. 親Issueの「子Issue」に `- [ ] #{子Issue番号}` を追記する
4. 子Issueの作業が完了したら、親Issueのチェックを入れる
5. APIの場合は、完了した子Issueの内容を親Issueの「詳細」にも反映する
6. frontの画面の場合は、完了した子Issueのファイルの配置場所を親Issueにも追記する

---

## 親Issue（汎用） {#parent-generic}

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 詳細

（この親Issueで何をまとめるか。1〜2行。必要なら複数行でもよい）

## 子Issue

- [ ] #

※ 子Issueの下には、必要に応じて一覧などの内容を追記してよい（例：CSS一覧）

</div>

### コピー用Markdown

````markdown
## 詳細
（この親Issueで何をまとめるか。1〜2行）

## 子Issue
- [ ] #
````

<details markdown="1">
<summary>AI向け補足（汎用の親Issue）</summary>

````markdown
# 汎用の親Issue作成ルール（AI向け）

GitHubのIssueの内容を作成する。親Issue1つに対して、複数の子Issueが紐づく。
機能ごとにIssueをまとめて管理しやすくすることが目的である。

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- （バック）Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 出力ルール
- Issue名を先に出し、その下にIssue本文をMarkdownのコードブロックで出力する。
- templateのfront matter（`---`で囲まれた `name` / `about` / `title` / `labels`）は出力しない。
- 項目はすべて書く必要はない。不要な項目は省略してよい。
- 子Issueの番号が未定の場合は `#` とだけ書く。

## 書き方
- Issue名は、機能名など汎用的な名前のみを書く（例：認証機能、ユーザ情報管理、SQL管理）。
- 見出しは「詳細」「子Issue」の順にする。
- 「詳細」は、この親Issueで何をまとめるかを書く。1〜2行を基本とし、必要なら複数行にしてよい。
- 「子Issue」は `- [ ] #{子Issue番号}` の形式で書く。
- 「子Issue」の下に、必要に応じて一覧などの内容を追記してよい（例：フロントのカスタムCSS管理なら、CSS一覧）。

## テンプレート
```markdown
## 詳細
（この親Issueで何をまとめるか。1〜2行）

## 子Issue
- [ ] #
```

## 運用ルール
- 子Issueを追加したら、親Issueの「子Issue」に `- [ ] #{子Issue番号}` を追記する。
- 子Issueの作業が完了したら、親Issueのチェックを入れる。
````

</details>

---

## 子Issue（汎用） {#child-generic}

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 親Issue

#

## 概要

（このIssueで何をするか。1〜2行）

## 詳細

（詳しい仕様・補足）

## 手順

### 1.

### 2.

## 完了条件

- [ ]

## その他

### ブランチ名

`feature/{issue番号}_{機能名}_{ユーザー名}`

</div>

### コピー用Markdown

````markdown
## 親Issue
#

## 概要

## 詳細

## 手順

### 1. 

### 2. 

## 完了条件
- [ ] 

## その他
### ブランチ名
`feature/{issue番号}_{機能名}_{ユーザー名}`
````

<details markdown="1">
<summary>AI向け補足（汎用の子Issue）</summary>

````markdown
# 汎用の子Issue作成ルール（AI向け）

GitHubのIssueの内容を作成する。子Issueは、親Issueにぶら下げる個別タスクである。
修正や仕様追加があるたびに、新しい子Issueを作成する。

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- （バック）Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 出力ルール
- Issue名も出す。その下にIssue本文をMarkdownのコードブロックで出力する。
- templateのfront matterは出力しない。
- 項目はすべて書く必要はない。不要な項目は省略してよい。
- 親Issueの番号が未定の場合は `#` とだけ書く。

## 書き方
- Issue名は、具体的な内容を書く（例：認証機能の作成、認証機能の更新）。
- 見出しは「親Issue → 概要 → 詳細 → 手順 → 完了条件 → その他」の順にする。
- 「親Issue」は `#{親Issue番号}` と書く。
- 「概要」は、何をするかを1〜2行で書く。
- 「詳細」は、このIssueで何をするかを書く。
- 「手順」は、`### 1.` `### 2.` のように番号付きの見出しで書く。コード例は `<details>` の「参考コード」にまとめてよい。
- 「完了条件」は、チェックボックス `- [ ]` で書く。
- 「その他」には、ブランチ名を書く。形式は `feature/{issue番号}_{機能名}_{ユーザー名}` とする。

## テンプレート
```markdown
## 親Issue
#

## 概要

## 詳細

## 手順

### 1. 

### 2. 

## 完了条件
- [ ] 

## その他
### ブランチ名
`feature/{issue番号}_{機能名}_{ユーザー名}`
```

## 運用ルール
- 子Issueの作業が完了したら、親Issueのチェックを入れる。
````

</details>

---

## 親Issue（API・CRUD） {#parent-api}

API（機能）単位の親Issueです。Issue名は機能名のみ（例：イベント作成機能）。

- 構成は「概要 → 子Issue → 詳細」です。
- 概要・子Issueはテンプレートでは空です。
- 詳細には、Swagger UIのURLと補足、自作する例外、HttpStatusごとの返却内容を書きます。
- 詳細な項目や説明はSwagger UIに書くため、Issueには細かく書きすぎません。

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 概要

## 子Issue

## 詳細

詳細は以下のSwagger UIに準ずる。

- Swagger UI:
- 詳細を書きます

### APIURL
url:

### 権限及びresponseするロール一覧

| ロール | Spring Securityの権限名 | 意味 |
| --- | --- | --- |
| `ADMIN` | `ROLE_ADMIN` | 管理者 |
| `STUDENT` | `ROLE_STUDENT` | 生徒 |
| `EXECUTIVE_CHAIR` | `ROLE_EXECUTIVE_CHAIR` | 実行委員長 |
| `BOOTH_LEADER` | `ROLE_BOOTH_LEADER` | ブース責任者 |
| `BOOTH_STAFF` | `ROLE_BOOTH_STAFF` | ブース担当者 |

### 業務ルールチェック（自作の例外）

| チェック内容 | errorCode |
| --- | --- |
|  |  |

### ステータス

#### Swagger UIに記載するステータス（必要に応じて編集する）

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 200 / 201 | - | {レスポンスDTO} | {操作}成功 |
| 400 | VALIDATION_ERROR | ValidationErrorResponse | MethodArgumentNotValidException：@Valid のバリデーションエラーの場合。DTOにバリデーションアノテーションを書き、Controllerの引数に @Valid を付ける。messageは日本語で指定する |
| 413 | PAYLOAD_TOO_LARGE | ErrorResponse | MaxUploadSizeExceededException：アップロードファイルのサイズが上限を超えた場合。自動で413にする |

#### Swagger UIにに記載しないが実装時意識すること

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。新しいAPIのURLを RoleRules に登録する（忘れると403になる）。許可するロールの組み合わせも選ぶ |
| 403 | PERMISSION_DENIED | ErrorResponse | PermissionDeniedException：認可を通った後の業務ルールによる拒否の場合（例：確定済みで編集不可）。拒否する箇所でthrowする |



#### アプリ側で自動的に返すステータス（共通で自動。何もしなくてよい）

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 400 | INVALID_REQUEST_BODY | ErrorResponse | HttpMessageNotReadableException：JSON形式不正・型不一致の場合。自動で400にする |
| 400 | MISSING_PARAMETER | ErrorResponse | MissingServletRequestParameterException：クエリの必須パラメータが欠落している場合。自動で400にする |
| 400 | INVALID_PARAMETER_TYPE | ErrorResponse | MethodArgumentTypeMismatchException：/events/abc のような型違いの場合。自動で400にする |
| 400 | INVALID_PARAMETER | ErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く。自動で400にする（@Valid と併用したメソッドでは @Valid 側のエラーもこちらで返る） |
| 400 | MISSING_PART | ErrorResponse | MissingServletRequestPartException：multipartで必須パートが無い場合。自動で400にする |
| 401 | UNAUTHORIZED | ErrorResponse | JwtAuthenticationEntryPoint：未ログイン・JWTの期限切れ・不正の場合。フィルター段階で返すため、例外は投げない |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。フィルター由来は JwtAccessDeniedHandler が、Controller・Service内で投げた場合は GlobalExceptionHandler が自動で403にする |
| 404 | RESOURCE_NOT_FOUND | ErrorResponse | NoResourceFoundException：存在しないURLへのアクセスの場合。自動で404にする |
| 415 | UNSUPPORTED_MEDIA_TYPE | ErrorResponse | HttpMediaTypeNotSupportedException：multipartやJSONの指定違い（Content-Typeの違い）の場合。自動で415にする |
| 500 | INTERNAL_SERVER_ERROR | ErrorResponse | Exception：想定外のエラーの場合。上記以外のすべてを自動で500にする |

</div>

### コピー用Markdown

````markdown
## 概要

## 子Issue

## 詳細
詳細は以下のSwagger UIに準ずる。
- Swagger UI: 
- 詳細を書きます

### APIURL
url:

### 業務ルールチェック（自作の例外）
| チェック内容 | errorCode |
| --- | --- |
|  |  |

### 権限及びresponseするロール一覧

| ロール | Spring Securityの権限名 | 意味 |
| --- | --- | --- |
| `ADMIN` | `ROLE_ADMIN` | 管理者 |
| `STUDENT` | `ROLE_STUDENT` | 生徒 |
| `EXECUTIVE_CHAIR` | `ROLE_EXECUTIVE_CHAIR` | 実行委員長 |
| `BOOTH_LEADER` | `ROLE_BOOTH_LEADER` | ブース責任者 |
| `BOOTH_STAFF` | `ROLE_BOOTH_STAFF` | ブース担当者 |

### ステータス

#### Swagger UIに記載するステータス（必要に応じて編集する）
| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 200 / 201 | - | {レスポンスDTO} | {操作}成功 |
| 400 | VALIDATION_ERROR | ValidationErrorResponse | MethodArgumentNotValidException：@Valid のバリデーションエラーの場合。DTOにバリデーションアノテーションを書き、Controllerの引数に @Valid を付ける。messageは日本語で指定する |
| 413 | PAYLOAD_TOO_LARGE | ErrorResponse | MaxUploadSizeExceededException：アップロードファイルのサイズが上限を超えた場合。自動で413にする |

#### Swagger UIにに記載しないが実装時意識すること

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。新しいAPIのURLを RoleRules に登録する（忘れると403になる）。許可するロールの組み合わせも選ぶ |
| 403 | PERMISSION_DENIED | ErrorResponse | PermissionDeniedException：認可を通った後の業務ルールによる拒否の場合（例：確定済みで編集不可）。拒否する箇所でthrowする |



#### アプリ側で自動的に返すステータス（共通で自動。何もしなくてよい）
| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 400 | INVALID_REQUEST_BODY | ErrorResponse | HttpMessageNotReadableException：JSON形式不正・型不一致の場合。自動で400にする |
| 400 | MISSING_PARAMETER | ErrorResponse | MissingServletRequestParameterException：クエリの必須パラメータが欠落している場合。自動で400にする |
| 400 | INVALID_PARAMETER_TYPE | ErrorResponse | MethodArgumentTypeMismatchException：/events/abc のような型違いの場合。自動で400にする |
| 400 | INVALID_PARAMETER | ErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く。自動で400にする（@Valid と併用したメソッドでは @Valid 側のエラーもこちらで返る） |
| 400 | MISSING_PART | ErrorResponse | MissingServletRequestPartException：multipartで必須パートが無い場合。自動で400にする |
| 401 | UNAUTHORIZED | ErrorResponse | JwtAuthenticationEntryPoint：未ログイン・JWTの期限切れ・不正の場合。フィルター段階で返すため、例外は投げない |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。フィルター由来は JwtAccessDeniedHandler が、Controller・Service内で投げた場合は GlobalExceptionHandler が自動で403にする |
| 404 | RESOURCE_NOT_FOUND | ErrorResponse | NoResourceFoundException：存在しないURLへのアクセスの場合。自動で404にする |
| 415 | UNSUPPORTED_MEDIA_TYPE | ErrorResponse | HttpMediaTypeNotSupportedException：multipartやJSONの指定違い（Content-Typeの違い）の場合。自動で415にする |
| 500 | INTERNAL_SERVER_ERROR | ErrorResponse | Exception：想定外のエラーの場合。上記以外のすべてを自動で500にする |
````

<details markdown="1">
<summary>書き方の例（イベント作成機能）</summary>

**Issue名:** イベント作成機能

````markdown
## 概要
イベント作成機能に関するAPIを管理する。
リクエスト・レスポンスのクラス(DTO)と、バリデーション、ステータスごとの返却内容を定義する。
修正や仕様追加の際は子Issueを作成し、下記の内容に追記する。

## 子Issue
- [ ] #71

## 詳細
詳細は以下のSwagger UIに準ずる。
- Swagger UI: http://localhost:8080/swagger-ui/index.html#/%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88/createEvent
- リクエストは `multipart/form-data`（画像ファイルを直接送信する）
- 画像は保存後、そのURLをDBの `image_url` に保存し、レスポンスの `imageUrl` で返す
- 画像の仕様：jpeg・png・webp、5MB以下

### 権限及びresponseするロール一覧

| ロール | Spring Securityの権限名 | 意味 |
| --- | --- | --- |
| `ADMIN` | `ROLE_ADMIN` | 管理者 |
| `STUDENT` | `ROLE_STUDENT` | 生徒 |
| `EXECUTIVE_CHAIR` | `ROLE_EXECUTIVE_CHAIR` | 実行委員長 |
| `BOOTH_LEADER` | `ROLE_BOOTH_LEADER` | ブース責任者 |
| `BOOTH_STAFF` | `ROLE_BOOTH_STAFF` | ブース担当者 |

### 業務ルールチェック（自作の例外）
| チェック内容 | errorCode |
| --- | --- |
| 同名のイベントが登録済みでないこと | EVENT_ALREADY_EXISTS |
| 画像のサイズが5MB以下であること | PAYLOAD_TOO_LARGE |
| 画像がjpeg・png・webpのいずれかであること | INVALID_IMAGE_FILE |

### ステータス
| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 201 | - | EventCreateResponse | イベント作成成功 |
| 400 | VALIDATION_ERROR | ValidationErrorResponse | `@Valid`のバリデーションエラー |
| 401 | UNAUTHORIZED | ErrorResponse | 未ログイン・セッション切れ |
| 409 | EVENT_ALREADY_EXISTS | ValidationErrorResponse | 同名のイベントが登録済みの場合 |
| 413 | PAYLOAD_TOO_LARGE | ErrorResponse | 画像のサイズが上限（5MB）を超えた場合 |
| 422 | INVALID_IMAGE_FILE | ValidationErrorResponse | 画像の形式が不正な場合 |
| 500 | INTERNAL_SERVER_ERROR | ErrorResponse | 想定外のエラー |

#### Swagger UIにに記載しないが実装時意識すること

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。新しいAPIのURLを RoleRules に登録する（忘れると403になる）。許可するロールの組み合わせも選ぶ |
| 403 | PERMISSION_DENIED | ErrorResponse | PermissionDeniedException：認可を通った後の業務ルールによる拒否の場合（例：確定済みで編集不可）。拒否する箇所でthrowする |

````

</details>

<details markdown="1">
<summary>AI向け補足（API親Issue）</summary>

````markdown
# API親Issue（CRUD）作成ルール（AI向け）

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 構成
- Issue名は機能名のみを書く（例：イベント作成機能）。templateのfront matterは出力しない。
- 見出しは「概要 → 子Issue → 詳細」の順にする。
- 概要：何を管理するか・子Issueの追加ルールを2〜3行で書く。
- 子Issue：`- [ ] #{子Issue番号}` の形式で書く（番号が未定の場合は `- [ ] #`）。
- 詳細：次を書く。
  - 冒頭に「詳細は以下のSwagger UIに準ずる。」と、Swagger UIのURLを書く。
    - URLの形式は `{ベースURL}/swagger-ui/index.html#/{@Tagのname(URLエンコード)}/{Controllerのメソッド名}`
  - その下に、リクエスト形式（ファイルを受け取る場合は `multipart/form-data`）・ファイルの保存方法・ファイルの仕様などの補足を箇条書きで書く。
  - 「業務ルールチェック（自作の例外）」の表（列は `チェック内容 | errorCode`）。
  - 「ステータス」の表。
- 詳細な項目や説明はSwagger UIに書くため、Issueには細かく書きすぎない。

## 自作の例外について
- 自作の例外とは、独自のフィルターと例外（`ValidationException`・`BusinessException`を継承したもの）を使ってキャッチする仕組みで返すエラーのこと。
- 詳しくは、Exception管理の親Issue（仕様書）を参照する。
- 業務ルールチェックの表の errorCode は、ステータスの表の行と対応させる。

## ステータスの書き方
ステータスは、次の2つの表に分けて書く。

1. **Swagger UIに記載するステータス**（APIごとに対応が必要。気にして書くこと）
   - 下記の「気にして書くこと」の表を最初から全部書き、そのAPIに合わせて編集する。
   - 不要な行は削除し、そのAPI固有の行（例：409・413・422など）は追加・書き換えをする。
   - 正常系（200 / 201）の行を先頭に書く。
2. **アプリ側で自動的に返すステータス**（共通で自動。気にしないこと）
   - フロント側で共通関数を使って対応するため、APIごとの対応は不要。
   - 下記の「気にしないこと」の表を、そのまま書く。

### 気にして書くこと（APIごとに対応が必要）

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 400 | VALIDATION_ERROR | ValidationErrorResponse | MethodArgumentNotValidException：@Valid のバリデーションエラーの場合。DTOにバリデーションアノテーションを書き、Controllerの引数に @Valid を付ける。messageは日本語で指定する |
| 413 | PAYLOAD_TOO_LARGE | ErrorResponse | MaxUploadSizeExceededException：アップロードファイルのサイズが上限を超えた場合。自動で413にする |

#### Swagger UIにに記載しないが実装時意識すること

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。新しいAPIのURLを RoleRules に登録する（忘れると403になる）。許可するロールの組み合わせも選ぶ |
| 403 | PERMISSION_DENIED | ErrorResponse | PermissionDeniedException：認可を通った後の業務ルールによる拒否の場合（例：確定済みで編集不可）。拒否する箇所でthrowする |


### 気にしないこと（共通で自動、何もしなくてよい）

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 400 | INVALID_REQUEST_BODY | ErrorResponse | HttpMessageNotReadableException：JSON形式不正・型不一致の場合。自動で400にする |
| 400 | MISSING_PARAMETER | ErrorResponse | MissingServletRequestParameterException：クエリの必須パラメータが欠落している場合。自動で400にする |
| 400 | INVALID_PARAMETER_TYPE | ErrorResponse | MethodArgumentTypeMismatchException：/events/abc のような型違いの場合。自動で400にする |
| 400 | INVALID_PARAMETER | ErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く。自動で400にする（@Valid と併用したメソッドでは @Valid 側のエラーもこちらで返る） |
| 400 | MISSING_PART | ErrorResponse | MissingServletRequestPartException：multipartで必須パートが無い場合。自動で400にする |
| 401 | UNAUTHORIZED | ErrorResponse | JwtAuthenticationEntryPoint：未ログイン・JWTの期限切れ・不正の場合。フィルター段階で返すため、例外は投げない |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。フィルター由来は JwtAccessDeniedHandler が、Controller・Service内で投げた場合は GlobalExceptionHandler が自動で403にする |
| 404 | RESOURCE_NOT_FOUND | ErrorResponse | NoResourceFoundException：存在しないURLへのアクセスの場合。自動で404にする |
| 415 | UNSUPPORTED_MEDIA_TYPE | ErrorResponse | HttpMediaTypeNotSupportedException：multipartやJSONの指定違い（Content-Typeの違い）の場合。自動で415にする |
| 500 | INTERNAL_SERVER_ERROR | ErrorResponse | Exception：想定外のエラーの場合。上記以外のすべてを自動で500にする |
````

</details>

---

## 子Issue（API・CRUD） {#child-api}

API（機能）の親Issueにぶら下げる、作成・更新・削除などの子Issueです。Issue名の例：イベント作成機能の作成。

- 構成は「親Issue → 概要 → 詳細 → 手順 → 完了条件 → その他」です。
- 「詳細」までは、親Issue（API・CRUD）と同じです。
- 手順は詳しく書きません。「3.」には、自分が行う手順を書き足します。
- Controller（仮実装）・DTO・ApiDocはSwagger UIの仕様書を作る段階で作成済みのため、手順は「追記する」と書きます。

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 親Issue

#

## 概要

## 詳細

詳細は親Issueの Swagger UI に準ずる。

- Swagger UI:
- 詳細を書きます

### 権限及びresponseするロール一覧

| ロール | Spring Securityの権限名 | 意味 |
| --- | --- | --- |
| `ADMIN` | `ROLE_ADMIN` | 管理者 |
| `STUDENT` | `ROLE_STUDENT` | 生徒 |
| `EXECUTIVE_CHAIR` | `ROLE_EXECUTIVE_CHAIR` | 実行委員長 |
| `BOOTH_LEADER` | `ROLE_BOOTH_LEADER` | ブース責任者 |
| `BOOTH_STAFF` | `ROLE_BOOTH_STAFF` | ブース担当者 |


### 業務ルールチェック（自作の例外）

| チェック内容 | errorCode |
| --- | --- |
|  |  |

### ステータス

#### Swagger UIに記載するステータス（必要に応じて編集する）

（親Issue（API・CRUD）と同じ表）

#### アプリ側で自動的に返すステータス（共通で自動。何もしなくてよい）

（親Issue（API・CRUD）と同じ表）

## 手順

### 0. 前提

Swagger UIに必要な項目が出来ている前提。

### 1. DTOにアノテーションを追記する

### 2. 例外を追加する

### 3. ======== 自分が行う手順をここに追記します ========

### 4. Controllerに追記する

### 5. RoleRules.javaに追記する

### 6. Swagger UIで動作確認する

### 7. 親Issueに自作例外の内容を追記する

## 完了条件

- [ ]

## その他

### ブランチ名

`feature/{子Issue番号}_{機能名}_{ユーザー名}`

</div>

### コピー用Markdown

````markdown
## 親Issue
#

## 概要

## 詳細
詳細は親Issueの Swagger UI に準ずる。
- Swagger UI: 
- 詳細を書きます

### 権限及びresponseするロール一覧

| ロール | Spring Securityの権限名 | 意味 |
| --- | --- | --- |
| `ADMIN` | `ROLE_ADMIN` | 管理者 |
| `STUDENT` | `ROLE_STUDENT` | 生徒 |
| `EXECUTIVE_CHAIR` | `ROLE_EXECUTIVE_CHAIR` | 実行委員長 |
| `BOOTH_LEADER` | `ROLE_BOOTH_LEADER` | ブース責任者 |
| `BOOTH_STAFF` | `ROLE_BOOTH_STAFF` | ブース担当者 |

### 業務ルールチェック（自作の例外）
| チェック内容 | errorCode |
| --- | --- |
|  |  |

### ステータス

#### Swagger UIに記載するステータス（必要に応じて編集する）
| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 200 / 201 | - | {レスポンスDTO} | {操作}成功 |
| 400 | VALIDATION_ERROR | ValidationErrorResponse | MethodArgumentNotValidException：@Valid のバリデーションエラーの場合。DTOにバリデーションアノテーションを書き、Controllerの引数に @Valid を付ける。messageは日本語で指定する |
| 413 | PAYLOAD_TOO_LARGE | ErrorResponse | MaxUploadSizeExceededException：アップロードファイルのサイズが上限を超えた場合。自動で413にする |

#### Swagger UIにに記載しないが実装時意識すること

| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。新しいAPIのURLを RoleRules に登録する（忘れると403になる）。許可するロールの組み合わせも選ぶ |
| 403 | PERMISSION_DENIED | ErrorResponse | PermissionDeniedException：認可を通った後の業務ルールによる拒否の場合（例：確定済みで編集不可）。拒否する箇所でthrowする |



#### アプリ側で自動的に返すステータス（共通で自動。何もしなくてよい）
| HttpStatus | errorCode | レスポンス | 用途 |
| --- | --- | --- | --- |
| 400 | INVALID_REQUEST_BODY | ErrorResponse | HttpMessageNotReadableException：JSON形式不正・型不一致の場合。自動で400にする |
| 400 | MISSING_PARAMETER | ErrorResponse | MissingServletRequestParameterException：クエリの必須パラメータが欠落している場合。自動で400にする |
| 400 | INVALID_PARAMETER_TYPE | ErrorResponse | MethodArgumentTypeMismatchException：/events/abc のような型違いの場合。自動で400にする |
| 400 | INVALID_PARAMETER | ErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く。自動で400にする（@Valid と併用したメソッドでは @Valid 側のエラーもこちらで返る） |
| 400 | MISSING_PART | ErrorResponse | MissingServletRequestPartException：multipartで必須パートが無い場合。自動で400にする |
| 401 | UNAUTHORIZED | ErrorResponse | JwtAuthenticationEntryPoint：未ログイン・JWTの期限切れ・不正の場合。フィルター段階で返すため、例外は投げない |
| 403 | FORBIDDEN | ErrorResponse | AccessDeniedException：ロール不足・RoleRules 未登録のURLの場合。フィルター由来は JwtAccessDeniedHandler が、Controller・Service内で投げた場合は GlobalExceptionHandler が自動で403にする |
| 404 | RESOURCE_NOT_FOUND | ErrorResponse | NoResourceFoundException：存在しないURLへのアクセスの場合。自動で404にする |
| 415 | UNSUPPORTED_MEDIA_TYPE | ErrorResponse | HttpMediaTypeNotSupportedException：multipartやJSONの指定違い（Content-Typeの違い）の場合。自動で415にする |
| 500 | INTERNAL_SERVER_ERROR | ErrorResponse | Exception：想定外のエラーの場合。上記以外のすべてを自動で500にする |

## 手順

### 0. 前提
Swagger UIに必要な項目が出来ている前提。

### 1. DTOにアノテーションを追記する

### 2. 例外を追加する

### 3. ======== 自分が行う手順をここに追記します ========

### 4. Controllerに追記する

### 5. RoleRules.javaに追記する

### 6. Swagger UIで動作確認する

### 7. 親Issueに自作例外の内容を追記する

## 完了条件
- [ ] 

## その他
### ブランチ名
`feature/{子Issue番号}_{機能名}_{ユーザー名}`
````

<details markdown="1">
<summary>書き方の例（イベント作成機能の作成）</summary>

**Issue名:** イベント作成機能の作成

自分が行う手順（3.）には、「Repository・Serviceを作成する」だけを書いています。

````markdown
## 親Issue
#70

## 概要
イベント作成APIを新規に実装する。
画像ファイルを直接受け取って保存し、そのURLをDBに保存する。

## 詳細
詳細は親Issueの Swagger UI(http://localhost:8080/swagger-ui/index.html#/%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88/createEvent) に準ずる。
- リクエストは `multipart/form-data`（画像ファイルを直接送信する）
- 画像は保存後、そのURLをDBの `image_url` に保存し、レスポンスの `imageUrl` で返す
- 画像の仕様：jpeg・png・webp、5MB以下

### 業務ルールチェック（自作の例外）
| チェック内容 | errorCode |
| --- | --- |
| 同名のイベントが登録済みでないこと | EVENT_ALREADY_EXISTS |
| 画像のサイズが5MB以下であること | PAYLOAD_TOO_LARGE |
| 画像がjpeg・png・webpのいずれかであること | INVALID_IMAGE_FILE |

### ステータス
（親Issueと同じ表）

## 手順

### 0. 前提
Swagger UIに必要な項目が出来ている前提。

### 1. DTOにアノテーションを追記する
作成済みの`EventCreateRequest`・`EventCreateResponse`にアノテーションを追記する。

### 2. 例外を追加する
`EventAlreadyExistsException`・`ImageTooLargeException`・`InvalidImageFileException`を追加する。

### 3. Repository・Serviceを作成する

### 4. Controllerに追記する
作成済みの`EventController`に、`EventService`の呼び出しを追記する。

### 5. RoleRules.javaに追記する
GET /api/events/{eventId}/studentsを、管理者のみ許可するルールに追記する。

### 5. Swagger UIで動作確認する
正常系と各エラーステータスが想定通り返ることを確認する。

### 6. 親Issueに自作例外の内容を追記する

## 完了条件
- [ ] イベント作成APIが201で`EventCreateResponse`を返す
- [ ] 各エラーステータス（400・409・413・422）が想定通り返る
- [ ] Swagger UIにAPIが反映されている
- [ ] 親Issueに自作例外の内容を追記した

## その他
### ブランチ名
`feature/71_event-create_{ユーザー名}`
````

</details>

<details markdown="1">
<summary>AI向け補足（API子Issue）</summary>

API親Issueの「AI向け補足」とあわせて、下記を渡す。

````markdown
# API子Issue（CRUD）作成ルール（AI向け）

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 構成
- Issue名も出す（例：イベント作成機能の作成）。templateのfront matterは出力しない。
- 見出しは「親Issue → 概要 → 詳細 → 手順 → 完了条件 → その他」の順にする。
- 親Issue：`#{親Issue番号}` と書く（番号が未定の場合は `#`）。
- 概要：何を実装するか・何を保存するかを2〜3行で書く。
- 詳細：親Issueの「詳細」と同じ書き方にする（Swagger UIのURL・補足・業務ルールチェック・ステータス）。
  - この子Issueで追加・変更する内容を中心に書く。
  - ステータスの表は、親Issueの2つの表（Swagger UIに記載するステータス・アプリ側で自動的に返すステータス）と同じ形で書く。

## 前提
- Controller（仮実装）・Request/Response DTO・ApiDocは、Swagger UIの仕様書を作る段階で作成済み。
- そのため、手順では「作成する」ではなく、「追記する」と書く。
- Swagger UIの作成手順は書かない。

## 手順
手順は、次の0〜6の順で書く。(場合によって - 出なく ### でもか)

- 0. 前提：Swagger UIに必要な項目が出来ている前提。
- 1. DTOにアノテーションを追記する
- 2. 例外を追加する
- 3. 自分が行う手順（このIssueごとに内容が変わる。例：Repository・Serviceを作成する）
- 4. Controllerに追記する
- 5. RoleRules.javaに追記(adminAndStudentRules() or adminExecutiveChairAndBoothRules())
- 5. Swagger UIで動作確認する
- 6. 親Issueに自作例外の内容を追記する

## 完了条件・その他
- 完了条件：各ステータスが想定通り返ること・Swagger UIへの反映・親Issueへの自作例外の追記を、チェックボックス `- [ ]` で書く。
- その他：ブランチ名を書く。形式は `feature/{子Issue番号}_{機能名}_{ユーザー名}` とする。

## 運用ルール
- 子Issueの作業が完了したら、親Issueの「詳細」に内容を反映し、「子Issue」のチェックを入れる。
````

</details>

<details markdown="1">
<summary>AI向け補足（Swagger UI用のコード：Controller・DTO・ApiDoc）</summary>

仕様書づくりの段階で、Issueを作る前にコードを作成するときに渡す。

````markdown
# Swagger UI用のコード（Controller・DTO・ApiDoc）の書き方（AI向け）

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 配置
- Controller：`controller/api/{機能名}Controller.java`
- Request DTO：`dto/request/{機能名}/{機能名}{操作}Request.java`
- Response DTO：`dto/response/{機能名}/{機能名}{操作}Response.java`
- ApiDoc：`doc/{機能名}ApiDoc.java`

## Controller
- `@Tag(name = ...)`・`@RestController`・`@RequestMapping`・`@RequiredArgsConstructor`を付ける。
- メソッドには、`@EventApiDoc.Post`のような、ApiDocのアノテーションと、`@PostMapping`を付ける。
- `multipart/form-data`の場合は、`@PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)`と`@Valid @ModelAttribute`を使う。
- Serviceを実装するまでは、`// 仮で設置`のコメントを付けて、`ResponseEntity.ok().build()`を返す。

## DTO
- RequestのDTOには、バリデーションアノテーションと、`message`（日本語）、`@Schema`を付ける。
- `multipart/form-data`の場合は、RequestのDTOに`@Setter`を付ける。
- `@Schema`には、バリデーションで表現されない仕様（ファイルの形式・サイズなど）は書かない。ApiDocに書く。
- ResponseのDTOには、`@Getter`・`@Builder`・`@Schema`を付ける。

## ApiDoc
- `{機能名}ApiDoc`クラスの中に、HTTPメソッド名のアノテーション（`@Post`・`@Get`・`@Put`・`@Delete`）を`@interface`で定義する。規約（config.md）に従う。
- `@Operation`の`description`に、必須項目・ファイルの仕様（形式・サイズ）・エラーになる条件を書く。
- `@ApiResponse`は、親Issueのステータスの表の行ごとに書く。`content`には、スキーマ（`ErrorResponse`・`ValidationErrorResponse`・Response DTO）と、例（`@ExampleObject`）を書く。
- 400の例には、バリデーションの対象になる全項目のエラーを含める。`errors`は、項目名とメッセージのMapにする。
- `multipart/form-data`の場合は、`@RequestBody`は書かず、springdocの自動生成に任せる。
````

</details>

---

## 親Issue（front・画面） {#parent-front}

画面単位の親Issueです。Issue名は画面名のみ（例：ログイン画面）。1画面につき、親Issueを1つ、子Issueを基本1つ作ります。

- 構成は「詳細 → 子Issue → 前提条件 → ディレクトリ構造 → ファイル一覧と役割 → 呼ぶAPI → 各ファイルの仕様 → その他 → 要確認」です。
- ディレクトリ構造・ファイル一覧・各ファイルの仕様は、[フロント開発規約](https://mbm-projects.github.io/document/front-document)に従って書きます。
- 各ファイルの仕様は、`<details>` で折りたたみます（長くなるため）。
- 部品が足りない場合は、部品の親Issueを別に立て、「前提条件」に書きます。
- 子Issueの終了時に、親Issueへファイルの配置場所を追記します。
- 実例：mbm-web-app の #131（ログイン画面）

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 詳細

（この画面で何をするか。参照するサンプル・規約）

## 子Issue

- [ ] #

## 前提条件

- [ ] （作成済みの部品・API・共通処理など）

## ディレクトリ構造

（`src/app/{画面名}/` 配下のtree図。作成済みの共通部品も載せる）

## ファイル一覧と役割

| ファイル | 種別 | 何をするか |
| --- | --- | --- |

## 呼ぶAPI

| 用途 | メソッド / パス | Orval 関数 | 呼び出し元 | レスポンス |
| --- | --- | --- | --- | --- |

## 各ファイルの仕様

▶ `page.tsx`（クリックで開く）

## その他

## 要確認

</div>

### コピー用Markdown

````markdown
## 詳細
（この画面で何をするか。参照するサンプル・規約）

## 子Issue
- [ ] #

## 前提条件
- [ ] （作成済みの部品・API・共通処理など）

## ディレクトリ構造
```
src/
├── app/
│   └── {画面名}/
│       ├── page.tsx
│       ├── _components/
│       │   └── {画面名}Form.tsx
│       ├── _lib/
│       │   ├── _constants/
│       │   ├── _hooks/
│       │   │   └── use{画面名}Form.ts
│       │   └── _services/
│       │       └── {動詞}{対象}.ts
│       └── _types/
│           └── index.ts
├── components/ui/common/                  # 作成済みの共通部品
├── constants/routes.ts
├── lib/services/                          # 作成済みの共通処理
└── api/openapi/generated/                 # Orval 生成（編集しない）
```

依存の向き：`page.tsx` → `_components` → `_hooks` → `_services`（→ `_types`、`_constants`）→ `generated`

## ファイル一覧と役割

| ファイル（`src/app/{画面名}/` 配下） | 種別 | 何をするか |
| --- | --- | --- |
| `page.tsx` | Server Component | ページの枠。見出しとフォームを置く |
| `_components/{画面名}Form.tsx` | Client Component | 画面のUI |
| `_lib/_hooks/use{画面名}Form.ts` | hooks | 状態管理と送信処理 |
| `_lib/_services/{動詞}{対象}.ts` | サービス | APIを呼ぶ |
| `_types/index.ts` | 型 | 生成型を受けて再公開する |

## 呼ぶAPI

| 用途 | メソッド / パス | Orval 関数 | 呼び出し元 | レスポンス |
| --- | --- | --- | --- | --- |
| | GET `/api/...` | | `_lib/_services/...` | 200：／400：／401：／403： |

## 各ファイルの仕様

<details>
<summary><code>page.tsx</code></summary>

- Server Component にする。`'use client'` は書かない
- 見出しとフォームの配置だけを書く。API 通信、hooks、型の処理は書かない
</details>

<details>
<summary><code>_components/{画面名}Form.tsx</code></summary>

- 先頭に `'use client'` を書く。1ファイル1コンポーネント
- 項目：

  | 項目 | 部品 | フォーム値 |
  | --- | --- | --- |
</details>

<details>
<summary><code>_lib/_hooks/use{画面名}Form.ts</code></summary>

- `useForm` を定義する。フォームの値の型はファイル上部に書く
- 送信処理、API呼び出し、エラーの反映を書く
</details>

<details>
<summary><code>_lib/_services/{動詞}{対象}.ts</code></summary>

- APIの呼び出しだけを書く（Orval 生成関数を呼び、ステータス判定とエラー制御を行う）
- 共通エラーは `lib/services/` の共通ハンドラに渡す
</details>

<details>
<summary><code>_types/index.ts</code></summary>

- `generated`（`index.ts` 経由）の型を re-export する
</details>

## その他

## 要確認
````

<details markdown="1">
<summary>AI向け補足（front親Issue）</summary>

````markdown
# front親Issue（画面）作成ルール（AI向け）

GitHubのIssueの内容を作成する。1画面につき、親Issueを1つ作り、子Issueを基本1つぶら下げる。

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document

## 出力ルール
- Issue名を先に出し、その下にIssue本文をMarkdownのコードブロックで出力する。
- templateのfront matter（`---`で囲まれた `name` / `about` / `title` / `labels`）は出力しない。
- Issue名は画面名のみを書く（例：ログイン画面）。ラベルは「親Issue」を付ける。
- 子Issueの番号が未定の場合は `#` とだけ書く。

## 構成
- 見出しは「詳細 → 子Issue → 前提条件 → ディレクトリ構造 → ファイル一覧と役割 → 呼ぶAPI → 各ファイルの仕様 → その他 → 要確認」の順にする。不要な項目は省略してよい。
- 詳細：この画面で何をするか、参照するサンプル・規約を書く。
- 子Issue：`- [ ] #{子Issue番号}` の形式で書く。
- 前提条件：作成済みの部品・API・共通処理を `- [x]`、未完了のものを `- [ ]` で書く。部品が足りない場合は、部品の親Issueを別に立てて書く。
- ディレクトリ構造：`src/app/{画面名}/` 配下のtree図を書く。作成済みの共通部品（`components/ui/common`、`constants/routes.ts`、`lib/services`、`api/openapi/generated`）も載せ、依存の向きを書く。
  - 規約：サブディレクトリは `_` 付き、`page.tsx` は Server Component、`'use client'` は `_components/` に書く、1ファイル1コンポーネント・1ファイル1公開関数。
- ファイル一覧と役割：ファイルごとに、種別と「何をするか」を表で書く。
- 呼ぶAPI：用途、メソッドとパス、Orval 関数、呼び出し元、レスポンス（ステータスごと）を表で書く。パスと関数名は `openapi.json` と `src/api/openapi/generated/` を確認する。まだ無いAPIは「未生成」と書き、前提条件にも書く。
- 各ファイルの仕様：ファイルごとに `<details>` で折りたたみ、簡単な仕様を箇条書きで書く。
  - `<summary>` の中は、バッククォートではなく `<code>` を使う。
- 画面の細かいデザイン・エラー表示の置き場所は、子Issueに書く。
- 要確認：決まっていないこと、規約やサンプルと食い違うことを書く。

## 規約との対応
- `generated` のAPI通信関数は `_services` からのみ呼ぶ。型は `_types`（または `src/types/`）で受けて再公開する。
- `generated` から import するときは、必ず `index.ts` 経由にする。
- `_services` のファイル名は、規約では `○○Server.ts` / `○○Client.ts`。Server Action の場合など、サンプルと異なるときは「要確認」に書く。
- zod によるクライアント側検証は、使う場合のみ `_schemas` に書く。

## 運用ルール
- 子Issueを追加したら、親Issueの「子Issue」に `- [ ] #{子Issue番号}` を追記する。
- 子Issueの作業が完了したら、親Issueのチェックを入れ、ファイルの配置場所を追記する。
````

</details>

---

## 子Issue（front・画面） {#child-front}

画面の親Issue（front・画面）にぶら下げる、画面の実装などの子Issueです。Issue名の例：ログイン画面の実装。

- 構成は「親Issue → 概要 → 詳細 → ファイルと import・API → 画面の構成 → エラーメッセージの出し方 → 手順 → 完了条件 → その他」です。
- ファイルごとの仕様は親Issueに書き、子Issueでは「ファイルと import・API」を `<details>` で書きます。
- 画面の構成（部品・配置・色）は、デザインから書きます。大きさ（幅・高さ・文字サイズ）の数値は書きません（共通部品の既定とTailwindで調整します）。
- 実例：mbm-web-app の #130（ログイン画面の実装）

### 表示イメージ

<div markdown="1" style="border:1px solid #d0d7de;border-radius:6px;padding:4px 20px;background:#f6f8fa;overflow-x:auto;">

## 親Issue

#

## 概要

## 詳細

## ファイルと import・API

▶ `page.tsx`（クリックで開く）

## 画面の構成

（レイアウト図と、要素の表）

### エラーメッセージの出し方

## 手順

### 1.

## 完了条件

- [ ]

## その他

### ブランチ名

`feature/{子Issue番号}_{画面名}_{ユーザー名}`

</div>

### コピー用Markdown

````markdown
## 親Issue
#

## 概要
（この画面を実装する。1行）

## 詳細
親Issueの「ディレクトリ構造」「ファイル一覧と役割」「各ファイルの仕様」に従い、`src/app/{画面名}/` を作成する。
- 画面：`page.tsx`、`_components/...`
- ロジック：`_lib/_hooks/...`
- API 呼び出し：`_lib/_services/...`
- 定数・型：`_lib/_constants/...`、`_types/index.ts`
- 共通部品（`Input`、`Button` など）を使う

着手前に、親Issueの前提条件を確認する。

## ファイルと import・API

`@/` は `src/` を指す。ファイルはすべて `src/app/{画面名}/` 配下に作る（以下は `src/app/{画面名}/` からの相対パス）。

<details>
<summary><code>page.tsx</code></summary>

- import
  - `import { XxxForm } from "./_components/XxxForm";`
</details>

<details>
<summary><code>_lib/_services/xxx.ts</code></summary>

- API：GET `/api/...`
- import
  - `import { xxx } from "@/api/openapi/generated";`
  - `import { handleApiError } from "@/lib/services/handleApiError";`
</details>

## 画面の構成
デザイン：`{デザインファイルのパス}`（背景色など）。大きさは共通部品の既定に任せ、デザインに合わせて Tailwind で調整する。

```
（レイアウト図）
```

| 要素 | 置く場所 | 使う部品 | デザインの仕様（文言・色・配置） | フォーム値 |
| --- | --- | --- | --- | --- |

### エラーメッセージの出し方
デザインにエラー表示がない場合は、次のとおりにする。

| 起きること | どこに出すか | 内容 | 処理 |
| --- | --- | --- | --- |
| 400 `VALIDATION_ERROR`（項目ごとの入力エラー） | 該当する入力欄の直下 | API が返す項目別のメッセージ | `applyFieldErrors` で各項目にセットして表示する |
| 401 | | | |
| 403・5xx・通信失敗 | エラー画面（`ROUTES.ERROR`） | | `handleApiError` が遷移する |

## 手順

### 1. 型・定数の作成

### 2. API 呼び出しの作成

### 3. フォームのロジックの作成

### 4. 画面の作成

### 5. 動作確認

## 完了条件
- [ ] 画面が表示され、操作できる
- [ ] エラーが決めた場所に表示される
- [ ] ESLint が通る
- [ ] 親Issueにファイルの配置場所を追記した

## その他
### ブランチ名
`feature/{子Issue番号}_{画面名}_{ユーザー名}`
````

<details markdown="1">
<summary>AI向け補足（front子Issue）</summary>

front親Issueの「AI向け補足」とあわせて、下記を渡す。

````markdown
# front子Issue（画面）作成ルール（AI向け）

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document

## 出力ルール
- Issue名も出す（例：ログイン画面の実装）。templateのfront matterは出力しない。
- ラベルは、機能開発なら「機能開発」を付ける。「親Issue」は付けない。

## 構成
- 見出しは「親Issue → 概要 → 詳細 → ファイルと import・API → 画面の構成 → エラーメッセージの出し方 → 手順 → 完了条件 → その他」の順にする。不要な項目は省略してよい。
- 親Issue：`#{親Issue番号}` と書く。
- 概要：何を実装するかを1行で書く。
- 詳細：作るファイルを、画面・ロジック・API 呼び出し・定数と型に分けて並べる。仕様は親Issueの「各ファイルの仕様」を参照させ、重複して書かない。着手前に確認する親Issueの前提条件も書く。
- ファイルと import・API：ファイルごとに `<details>` で折りたたみ、使うAPI（メソッドとパス）と import 文を、パスつきで書く。
  - `@/` は `src/` を指す。import は `generated` の `index.ts` 経由にする。API 通信関数は `_services` からのみ呼ぶ。
  - 確定していない名前（関数名、配置先）は「仮」と書く。
  - `<summary>` の中は、バッククォートではなく `<code>` を使う。
- 画面の構成：デザイン（Figma・SVG など）を読み取り、レイアウト図と要素の表（置く場所、使う部品、文言・色・配置、フォーム値）を書く。
  - 大きさ（幅・高さ・文字サイズ・座標）の数値は書かない。共通部品の既定に任せ、Tailwind で調整する。
  - 共通部品が持たない見た目（variant など）は、親Issueの「要確認」に書く。
  - 部品は、実際に存在するもの（`src/components/ui/common/`）と、その Props を確認して選ぶ。用途の違う部品は使わない（例：`HelpText` は補足文用で、エラー表示には使えない）。
- エラーメッセージの出し方：400（項目ごと）、401、403、5xx・通信失敗ごとに、「どこに出すか」「内容」「処理」を表で書く。
  - `handleApiError` の挙動（401 は `ROUTES.LOGIN` へ遷移、403・その他は `ROUTES.ERROR` へ遷移、400 `VALIDATION_ERROR` は `fieldErrors` を返す）を確認し、画面に残したいメッセージと食い違う場合は「要確認」に書く。
- 手順：「型・定数 → API 呼び出し → フォームのロジック → 画面 → 動作確認」の順に、依存の下から上へ書く。サンプルをコピーする手順にはしない。
- 完了条件：画面の動作、エラー表示、ESLint、親Issueへの追記を、チェックボックス `- [ ]` で書く。
- その他：ブランチ名を書く。形式は `feature/{子Issue番号}_{画面名}_{ユーザー名}` とする。

## 運用ルール
- 子Issueの作業が完了したら、親Issueのチェックを入れ、ファイルの配置場所を親Issueに追記する。
- 画面デザインの確認事項は、親Issueの「要確認」に書く。
````

</details>

---

## API・CRUD 作成手順(今回はタスクの仕様書を作成するIssue) {#child-creation-steps}
### 1. タスク一覧に親Issueのタイトルを記述し、Issueを作成する
### 2. 親Issueのラベルを親Issueにする
### 3. 親Issueのtemplateをコピーして内容を決める
このAPIでは何をするのかを全体像を考えながら以下の項目を決めていく
#### 概要
このAPIがどんなことをするのかを簡単に記述する
#### 子Issue
このAPIタスクに関するタスクを記載する欄(作成タスク・アップロードタスク・修正タスク・テストタスク等)
#### 詳細
簡単にこのAPIの詳細を記述する
##### 必ず記載するもの
- Swagger UIのURL
##### 必要に応じて記載するもの
- リクエストボディの有無・レスポンスボディの有無・パラメータについて等
- 実装の際に気負つけておきたいこと(CSRFトークン返す・管理者だけですよ・事前に用意したこのサービス使用する等)
- 仕様に対すること(各セクションごとに何をするの簡単になど)
- その他必要に応じて

#### APIURL
APIのURLを記載する
#### 権限及びresponseするロール一覧
このAPIでレスポンスするロールを考えて記載する。  
テンプレートから必要ないのを削除すればよい。

#### 業務ルールチェック（自作の例外）
このAPIで下記のステータス一覧にない例外(あらかじめ用意されていない例外)を考えて内容とfrontが取得するエラーコードを記載する。  
例：ベースなるイベントが存在しない・flgがfalseなのでできない等

#### ステータス
下記のセクションで説明

### ステータス
このAPIで返すAPIを記載する
#### Swagger UIに記載するステータス
フロント側で認識したいステータスを記載する。  
テンプレートに記載されている項目から必要なもののみ残して業務ルールチェックで記載したものを追記する

#### Swagger UIに記載しないが実装時意識すること
UIには記載しないがバックエンドで実装するときにこのAPIで返るであろうステータスをテンプレに残す。
- FORBIDDENではロールがある場合レスポンスに含める・実装時に権限に気を付ける・RoleRulesに登録しない等
- PERMISSION_DENIEDでは認可のフィルター以外での権限エラーを返す場合にこのエスケプションをサービス内で投げるように実装する

#### アプリ側で自動的に返すステータス
アプリ側がフィルターなどで自動に返すレスポンス。  
UIには記載せず、front側でも自動でキャッチするので書かなくてよい。  
ただこのAPIで返らなそうなステータスの列は削除する(パブリックアクセスでは401削除するなど)

### 4. 子Issue(作成Issueを作成する)
親Issueの子Issueの欄にAPIの作成Issueを作成し、テンプレを張り付けて内容を消えていく

#### 親Issue
親IssueのIssue番号を記載する
#### 概要・詳細・権限及びresponseするロール一覧・業務ルールチェック（自作の例外）・ステータス
基本的に親Issueの内容と同じでいい(ステータスは親Issueと同じと記載でよい)  
詳細のリストに関しては作成手順で改めて記載することがあれば記載したほうがよい
<details markdown="1">
<summary>ステータスの省略例</summary>

#### Swagger UIに記載するステータス（必要に応じて編集する）
（親Issueと同じ表）

#### Swagger UIに記載しないが実装時意識すること
（親Issueと同じ表）

#### アプリ側で自動的に返すステータス（共通で自動。何もしなくてよい）
（親Issueと同じ表）


</details>


#### 手順
テンプレートの3番以外のにあ用は必要に応じて内容を追記する。  
======== 自分が行う手順をここに追記します =======では Mapper・Repository・Serviceを作成するなど簡単な粒度でよい。  
ここで例外投げるなど実装の際に気を付けることは記載した方が良い。

<details markdown="1">
<summary>手順例</summary>

## 手順

### 0. 前提
Swagger UIに必要な項目が出来ている前提。

### 1. DTOにアノテーションを追記する
作成済みの`EventOverviewResponse`にアノテーションを追記する。
`eventId`は`@PathVariable`のため、Controllerの引数にバリデーションアノテーションを付ける（messageは日本語）。

### 2. 例外を追加する
`EventNotFoundException`（404 / `EVENT_NOT_FOUND`）を`exception/event/`に追加する（作成済みの場合は流用する）。

### 3. Mapper・Repository・Serviceを作成する
serviceでイベントがない場合は`EventNotFoundException`（404 / `EVENT_NOT_FOUND`）をスローする

### 4. Controllerに追記する
作成済みの`EventController`に、`EventService`の呼び出しを追記する。

### 5. RoleRules.javaに追記する
`GET /api/events/{eventId}/overview`を、管理者のみ許可するルールに追記する。

### 6. Swagger UIで動作確認する
正常系と各エラーステータス（400・404）が想定通り返ることを確認する。
管理者以外のロールで403（`FORBIDDEN`）が返ることも確認する。

### 7. 親Issueに自作例外の内容を追記する


</details>

#### 完了条件
完了条件を入力する
このAPIで実装されているべき内容を記載する

#### その他
このタスクのブランチ名を決めたり、その他の留意点等を記載する


### 5.Swagger UIの内容を作成する
作成手順は以下のとおりである([https://mbm-projects.github.io/document/swagger-ui-template#crud-api-%E4%BB%95%E6%A7%98%E6%9B%B8%E4%BD%9C%E6%88%90issue%E3%81%A7%E3%81%AE%E4%BD%9C%E6%88%90%E6%89%8B%E9%A0%86](https://mbm-projects.github.io/document/swagger-ui-template#crud-api-%E4%BB%95%E6%A7%98%E6%9B%B8%E4%BD%9C%E6%88%90issue%E3%81%A7%E3%81%AE%E4%BD%9C%E6%88%90%E6%89%8B%E9%A0%86))

### 6.内容をpushしprを作成する

### 7.レビューを受ける


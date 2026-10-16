---
title: Issueテンプレート
---

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
| 400 | INVALID_PARAMETER | ValidationErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く |
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
| 400 | INVALID_PARAMETER | ValidationErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く |
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
| 400 | INVALID_PARAMETER | ValidationErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く |
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

### 5. Swagger UIで動作確認する

### 6. 親Issueに自作例外の内容を追記する

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
| 400 | INVALID_PARAMETER | ValidationErrorResponse | HandlerMethodValidationException：@PathVariable・@RequestParam のバリデーションエラーの場合。引数にバリデーションアノテーションを書く |
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

### 5. Swagger UIで動作確認する

### 6. 親Issueに自作例外の内容を追記する

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

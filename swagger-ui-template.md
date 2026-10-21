## Swagger UI用のコード（Controller・DTO・ApiDoc）の書き方 {#swagger-code}

仕様書づくりの段階で、Issueを作る前に、次のコードを先に作成します。
そのため、API子Issueの手順では、これらを「作成する」ではなく「追記する」「置き換える」と書きます。

### 配置

| 種類 | 配置 |
| --- | --- |
| Controller | `controller/api/{機能名}Controller.java` |
| Request DTO | `dto/request/{機能名}/{機能名}{操作}Request.java` |
| Response DTO | `dto/response/{機能名}/{機能名}{操作}Response.java` |
| ApiDoc | `doc/{機能名}ApiDoc.java` |

### Controller

- `@Tag(name = ...)`・`@RestController`・`@RequestMapping`・`@RequiredArgsConstructor`を付ける
- `@Tag(name=)`は日本語は記述してならない。next.jsのorvalで自動生成しているため
- メソッドには、`@EventApiDoc.Post`のような、ApiDocのアノテーションと、`@PostMapping`を付ける
- `multipart/form-data`の場合は、`@PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)`と`@Valid @ModelAttribute`を使う
- Serviceを実装するまでは、`// 仮で設置`のコメントを付けて、`ResponseEntity.ok().build()`を返す

### DTO

- RequestのDTOには、バリデーションアノテーションと、`message`（日本語）、`@Schema`を付ける
- `multipart/form-data`の場合は、RequestのDTOに`@Setter`を付ける
- `@Schema`には、バリデーションで表現されない仕様（ファイルの形式・サイズなど）は書かない。ApiDocに書く
- ResponseのDTOには、`@Getter`・`@Builder`・`@Schema`を付ける

### ApiDoc

- `{機能名}ApiDoc`クラスの中に、HTTPメソッド名のアノテーション（`@Post`・`@Get`・`@Put`・`@Delete`）を`@interface`で定義する。規約（config.md）に従う
- `@Operation`の`description`に、必須項目・ファイルの仕様（形式・サイズ）・エラーになる条件を書く
- `@ApiResponse`は、親Issueのステータスの表の行ごとに書く。`content`には、スキーマ（`ErrorResponse`・`ValidationErrorResponse`・Response DTO）と、例（`@ExampleObject`）を書く
- 400の例には、バリデーションの対象になる全項目のエラーを含める。`errors`は、項目名とメッセージのMapにする
- `multipart/form-data`の場合は、`@RequestBody`は書かず、springdocの自動生成に任せる

<details markdown="1">
<summary>AI向け補足（Swagger UI用のコード：Controller・DTO・ApiDoc）</summary>

仕様書づくりの段階で、Issueを作る前にコードを作成するときに渡す。

````markdown
# Swagger UI用のコード（Controller・DTO・ApiDoc）の書き方（AI向け）

## 開発規約
- フロント: https://mbm-projects.github.io/document/front-document
- バック: https://mbm-projects.github.io/document/back-document
- Spring Bootで環境変数を`@Value`ではなく、あるファイルに変数として定義して使う場合は、`@ConfigurationPropertiesScan`を使用する。

## 前提
- 仕様書づくりの段階で、Issueを作る前に、Controller・Request/Response DTO・ApiDocを先に作成する。
- Serviceはまだ実装しない。Controllerは仮実装にする。

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


<details markdown="1">
    
<summary>参考コード（Swagger UI用のコード：Controller・DTO・ApiDoc）</summary>

## controller

```java
package org.example.app.controller.api;

import org.example.app.doc.EventOverviewApiDoc;
import org.example.app.dto.response.eventoverview.EventOverviewResponse;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.validation.constraints.Min;
import lombok.RequiredArgsConstructor;

/**
 * イベント概要API
 */
@Tag(name = "EventOverview", description = "イベントの概要（イベント関連情報・申請全体情報・申請情報管理）に関するAPI")
@RestController
@RequestMapping("/api/events/{eventId}/overview")
@RequiredArgsConstructor
public class EventOverviewController {

    /**
     * イベント概要を取得する
     *
     * @param eventId
     *            イベントID
     * @return イベント概要レスポンス
     */
    @EventOverviewApiDoc.Get
    @GetMapping
    public ResponseEntity<EventOverviewResponse> getEventOverview(@PathVariable @Min(value = 1, message = "イベントIDは1以上を指定してください") int eventId) {
        // 仮で設置
        return ResponseEntity.ok().build();
    }
}
```

## doc

```java
package org.example.app.doc;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.example.app.dto.response.eventoverview.EventOverviewResponse;
import org.example.app.exception.ErrorResponse;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.enums.ParameterIn;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.ExampleObject;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;

/**
 * EventOverviewControllerのAPI仕様書定義
 */
public class EventOverviewApiDoc {

    /** イベント概要取得 */
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @Operation(summary = "イベント概要取得", description = """
        指定したイベントの概要を返す。

        - eventInfo（イベント関連情報）：ブース総数・参加生徒数・申請承認進捗（承認済み申請数 / 申請総数。例：1/34）
        - applicationSummary（イベント申請全体情報）：予算申請合計・予算確定金額合計・売上合計
        - applicationManagement（申請情報管理）：ブース申請・売上申請・備品申請・予算申請・予算使用申請それぞれの
          申請名・締め切り・注意事項・更新日
        - 締め切り・注意事項・更新日は、申請設定が未登録の場合 null を返す
        - eventId が1未満の場合は400を返す
        - 存在しないイベントの場合は404を返す
        - ログイン済みの管理者のみ実行できる
        """)
    @Parameter(name = "eventId", in = ParameterIn.PATH, description = "イベントID", example = "1")
    @ApiResponse(responseCode = "200", description = "取得成功", content = @Content(mediaType = "application/json", schema = @Schema(implementation = EventOverviewResponse.class), examples = @ExampleObject(name = "取得成功", value = """
        {
          "role": "ADMIN",
          "eventInfo": {
            "boothCount": 34,
            "studentCount": 120,
            "approvedApplicationCount": 1,
            "totalApplicationCount": 34
          },
          "applicationSummary": {
            "budgetApplicationTotal": 500000,
            "budgetConfirmedTotal": 420000,
            "salesTotal": 800000
          },
          "applicationManagement": {
            "boothApplication": {
              "applicationName": "ブース申請",
              "deadlineAt": "2026-10-31T23:59:00",
              "remarks": "レイアウト図を必ず添付してください。",
              "updatedAt": "2026-10-07T10:31:00"
            },
            "salesApplication": {
              "applicationName": "売上申請",
              "deadlineAt": "2026-11-05T23:59:00",
              "remarks": "金種ごとの枚数を入力してください。",
              "updatedAt": "2026-10-07T10:31:00"
            },
            "equipmentApplication": {
              "applicationName": "備品申請",
              "deadlineAt": null,
              "remarks": null,
              "updatedAt": null
            },
            "budgetApplication": {
              "applicationName": "予算申請",
              "deadlineAt": "2026-10-20T23:59:00",
              "remarks": "見積書を添付してください。",
              "updatedAt": "2026-10-07T10:31:00"
            },
            "budgetUsageApplication": {
              "applicationName": "予算使用申請",
              "deadlineAt": "2026-11-10T23:59:00",
              "remarks": "領収書の画像を添付してください。",
              "updatedAt": "2026-10-07T10:31:00"
            }
          }
        }
        """)))
    @ApiResponse(responseCode = "400", description = "パラメータ不正", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "パラメータ不正", value = """
        {
          "errorCode": "INVALID_PARAMETER",
          "message": "パラメータの形式が正しくありません"
        }
        """)))
    @ApiResponse(responseCode = "404", description = "イベントが存在しない", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "存在しない", value = """
        {
          "errorCode": "EVENT_NOT_FOUND",
          "message": "指定されたイベント(ID: 999)は存在しません"
        }
        """)))
    public @interface Get {
    }
}
```

## request（これのみ　別のものだが参考コードなので）

```java
package org.example.app.dto.response.event;

import java.time.LocalDateTime;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "イベント作成レスポンス")
public class EventCreateResponse {

    @Schema(description = "イベントID", example = "1", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer id;

    @Schema(description = "タイトル", example = "文化祭2026", requiredMode = Schema.RequiredMode.REQUIRED)
    private String title;

    @Schema(description = "詳細", example = "第10回文化祭を開催します。", requiredMode = Schema.RequiredMode.REQUIRED)
    private String detail;

    @Schema(description = "開催年（4桁の西暦）", example = "2026", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer year;

    @Schema(description = "開催日（表示用テキスト）", example = "2026年11月1日(日)・2日(月)", requiredMode = Schema.RequiredMode.REQUIRED)
    private String eventDatetimeText;

    @Schema(description = "画像URL（未設定の場合はnull）", example = "https://example.com/images/event-1.png", requiredMode = Schema.RequiredMode.REQUIRED, nullable = true)
    private String imageUrl;

    @Schema(description = "イベントURL（デジタルパンフレットURL。未設定の場合はnull）", example = "/2026-pamphlet", requiredMode = Schema.RequiredMode.REQUIRED, nullable = true)
    private String digitalPamphletUrl;

    @Schema(description = "ステータス（10:準備前。コード値は設計書のコード定義を参照）", example = "10", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer status;

    @Schema(description = "作成日時", example = "2026-10-07T10:31:00", requiredMode = Schema.RequiredMode.REQUIRED)
    private LocalDateTime createdAt;

    @Schema(description = "更新日時", example = "2026-10-07T10:31:00", requiredMode = Schema.RequiredMode.REQUIRED)
    private LocalDateTime updatedAt;
}
```

## response

```java
package org.example.app.dto.response.eventoverview;

import org.example.app.security.config.RoleType;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * イベント概要レスポンス
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "イベント概要レスポンス")
public class EventOverviewResponse {

    @Schema(description = "ロール（ADMIN か STUDENT）", example = "ADMIN", requiredMode = Schema.RequiredMode.REQUIRED)
    private RoleType role;

    @Schema(description = "イベント関連情報", requiredMode = Schema.RequiredMode.REQUIRED)
    private EventInfoDto eventInfo;

    @Schema(description = "イベント申請全体情報", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSummaryDto applicationSummary;

    @Schema(description = "申請情報管理", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationManagementDto applicationManagement;
}
```

</details>


## CRUD API 仕様書作成Issueでの作成手順
### 1. figma・仕様書・DB設計等からリクエスト・レスポンスで必要な値を考える
### 2. リクエストクラスを作成する(Request DTO)
以下のものを考え記述する
- 必要なフィールド
- 必要なバリデーション条件
- バリデーションごとにエラーメッセージ

### 3. レスポンスクラスを作成する(Response DTO)
以下のものを考え記述する  
- ロールごとにレスポンスの形の変更が必要か
- 必要なフィールド(roleは必ず含めること)
- `@Schema`の内容(細かい内容は下記に記載あり)
※ロールごとにレスポンスの内容(フィールド構成)が変わる場合は複数個クラスを作成する
#### `@Schema`属性について
**属性ごとの意味**

| 属性 | 意味 |
|---|---|
| `description` | フィールドの説明文。Swagger UIにそのまま表示される |
| `example` | サンプル値。Swagger UIの「Try it out」やリクエスト例に初期値として表示される |
| `requiredMode` | 必須かどうかの指定 |

**`example`の使い方について**

レスポンスの例は、DTOのフィールドに付ける `@Schema(example = ...)` から自動で生成されるのでサンプルの値を記載する。  


**基本：フィールドごとに値を1つ書く**

```java
@Schema(description = "ブース総数", example = "34", requiredMode = Schema.RequiredMode.REQUIRED)
private Integer boothCount;
```

- 文字列・数値・日時など、1つの値を `example` に指定する
- 日時は ISO 形式で書く（例：`"2026-10-31T23:59:00"`）
- 値が `null` になりうるフィールドでも、example には値のある例を書く

**例外：同じクラスを複数のフィールドで使い、値を変えたい場合**

同じクラス（例：`ApplicationSettingDto`）を複数のフィールドで使うと、クラス側の example が全フィールドに共通で表示される。
フィールドごとに値を変えたい場合は、使う側のフィールドの `example` に、オブジェクト全体を `{}` の JSON 文字列で書いて上書きする。

```java
@Schema(description = "売上申請",
    example = """
    {"applicationName":"売上申請","deadlineAt":"2026-11-05T23:59:00","remarks":"金種ごとの枚数を入力してください。","updatedAt":"2026-10-07T10:31:00"}""",
    requiredMode = Schema.RequiredMode.REQUIRED)
private ApplicationSettingDto salesApplication;
```

### 注意

- `{}` で書く場合は、一部の項目だけの上書きはできない。クラスのすべての項目を書く
- JSON の項目名は、クラスのフィールド名と一致させる
- 値を変える必要がなければ、`{}` は使わず、クラス側の example に任せる



**`requiredMode` の値**

`Schema.RequiredMode.REQUIRED` は「このフィールドは必須」という意味です。

| 値 | 意味 |
|---|---|
| `REQUIRED` | 必須として表示する |
| `NOT_REQUIRED` | 任意として表示する |
| `AUTO` | 自動判定(デフォルト) |

### 4. controllerを作成する
controllerでは以下のことを行う。

#### クラス名上アノテーションの設定
クラス名の上に以下のアノテーションを追加する

**Tag**

| 属性 | 意味 |
|---|---|
| `name` | グループの名前。Swagger UIの見出しになる。**TypeScriptのファイル名としてふさわしいアルファベットにする**(例: `event-overview`) |
| `description` | グループの説明文。見出しの横や下に表示される |

- `@RestController`
- `@RequestMapping`
- `@RequiredArgsConstructor`
- `@Validated`(引数にバリデーションを使う場合)

#### メソッド名上アノテーションの設定
メソッド名の上に以下のアノテーションを追加する

- `@{Doc名}.{HTTPメソッド名}`(例: `@EventOverviewApiDoc.Get`)
- HTTPメソッドに対応するマッピング(例: `@GetMapping`、`@PostMapping`)

#### 引数の設定
引数に必要なアノテーションを追加する

- パスの値を受け取る場合は `@PathVariable`
- クエリパラメータを受け取る場合は `@RequestParam`
- リクエストボディを受け取る場合は `@RequestBody @Valid`
- 必要に応じてバリデーション(例: `@Min`)を追加する

#### 戻り値の設定
- 戻り値は `ResponseEntity<レスポンスDTO>` にする
- Serviceを呼び出し、結果を `ResponseEntity.ok().body(...)` で返す
- Service未実装の間は `ResponseEntity.ok().build()` を仮で返す




### 5. Docを作成する
Docでは以下のことを行う。

#### クラスの作成
- `{機能名}ApiDoc` というクラスを作成する(例: `EventOverviewApiDoc`)
- クラス内に、エンドポイントごとの `@interface` を作成する(例: `Get`、`Post`、`Put`、`Delete`)
- 作成した `@interface` を、Controllerのメソッドに付けて使う

#### `@interface` の上のアノテーションの設定
`@interface` の上に以下のアノテーションを追加する

- `@Target(ElementType.METHOD)`:メソッドに付けられるようにする
- `@Retention(RetentionPolicy.RUNTIME)`:実行時にSwaggerが読み取れるようにする

**Operation**

| 属性 | 意味 |
|---|---|
| `summary` | APIの短いタイトル。Swagger UIの一覧に表示される |
| `description` | APIの詳細な説明。返す内容、nullになる条件、エラーになる条件、権限などを書く |

**Parameter**

| 属性 | 意味 |
|---|---|
| `name` | パラメータ名。引数名と一致させる |
| `in` | パラメータの場所(`ParameterIn.PATH`、`ParameterIn.QUERY` など) |
| `description` | パラメータの説明 |
| `example` | サンプル値 |

**ApiResponse**

| 属性 | 意味 |
|---|---|
| `responseCode` | HTTPステータスコード(`"200"`、`"404"` など) |
| `description` | レスポンスの説明 |
| `content` | レスポンスの中身。`@Content` で指定する |

**Content**

| 属性 | 意味 |
|---|---|
| `mediaType` | データ形式(`"application/json"` など) |
| `schema` | レスポンスの型。`@Schema(implementation = クラス名.class)` で指定する |
| `examples` | JSONの例。`@ExampleObject` で指定する(省略可) |

#### レスポンスごとの設定
- 成功(200など)は、`@ApiResponse` でレスポンスDTOを指定する
- そのAPI固有のエラー(404など)は、`@ApiResponse` で `ErrorResponse` を指定する
- 複数の `@ApiResponse` を並べて、ステータスコードごとに書く

#### 共通エラーの設定
- 複数のAPIで共通するエラーは、共通のDocにまとめて適用する
  - `@CommonErrorApiDoc.InvalidParameter`:400(パラメータ不正)
  - `@CommonErrorApiDoc.Authenticated`:401(未ログイン)・403(権限不足)・500(想定外のエラー)
- 共通エラーは、各APIで個別に書かなくてよい



<details markdown="1">
<summary>参考コード</summary>

#### doc

```java
package org.example.app.doc;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.example.app.doc.template.CommonErrorApiDoc;
import org.example.app.dto.response.eventoverview.EventOverviewResponse;
import org.example.app.exception.ErrorResponse;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.Parameter;
import io.swagger.v3.oas.annotations.enums.ParameterIn;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.ExampleObject;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;

/**
 * EventOverviewControllerのAPI仕様書定義
 */
public class EventOverviewApiDoc {

    /** イベント概要取得 */
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @Operation(summary = "イベント概要取得", description = """
        指定したイベントの概要を返す。。

        - eventInfo（イベント関連情報）：ブース総数・参加生徒数・申請承認進捗（承認済み申請数 / 申請総数。例：1/34）
        - applicationSummary（イベント申請全体情報）：予算申請合計・予算確定金額合計・売上合計
        - applicationManagement（申請情報管理）：ブース申請・売上申請・備品申請・予算申請・予算使用申請それぞれの
          申請名・締め切り・注意事項・更新日
        - 締め切り・注意事項・更新日は、申請設定が未登録の場合 null を返す
        - eventId が1未満の場合は400を返す
        - 存在しないイベントの場合は404を返す
        - ログイン済みの管理者のみ実行できる
        """)
    @Parameter(name = "eventId", in = ParameterIn.PATH, description = "イベントID", example = "1")
    @ApiResponse(responseCode = "200", description = "取得成功", content = @Content(mediaType = "application/json", schema = @Schema(implementation = EventOverviewResponse.class)))
    @ApiResponse(responseCode = "404", description = "イベントが存在しない", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "存在しない", value = """
        {
          "errorCode": "EVENT_NOT_FOUND",
          "message": "指定されたイベント(ID: 999)は存在しません"
        }
        """)))
    // 共通エラー：400（パラメータ不正）
    @CommonErrorApiDoc.InvalidParameter
    // 共通エラー：401（未ログイン）・403（権限不足）・500（想定外のエラー）
    @CommonErrorApiDoc.Authenticated
    public @interface Get {
    }
}
```

#### controller

```java
package org.example.app.controller.api;

import org.example.app.doc.EventOverviewApiDoc;
import org.example.app.dto.response.eventoverview.EventOverviewResponse;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.validation.constraints.Min;
import lombok.RequiredArgsConstructor;

/**
 * イベント概要API
 */
@Tag(name = "イベント概要", description = "イベントの概要（イベント関連情報・申請全体情報・申請情報管理）に関するAPI")
@RestController
@RequestMapping("/api/events/{eventId}/overview")
@RequiredArgsConstructor
public class EventOverviewController {

    /**
     * イベント概要を取得する
     *
     * @param eventId
     *            イベントID
     * @return イベント概要レスポンス
     */
    @EventOverviewApiDoc.Get
    @GetMapping
    public ResponseEntity<EventOverviewResponse> getEventOverview(@PathVariable @Min(value = 1, message = "イベントIDは1以上を指定してください") int eventId) {
        // 仮で設置
        return ResponseEntity.ok().build();
    }
}
```

#### request(LoginRequest)

```java
package org.example.app.dto.request;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.ToString;

/**
 * ログインリクエスト
 */
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
public class LoginRequest {

    /** ログインID（管理者はID、生徒はメールアドレスのため @Email は付けない） */
    @ToString.Exclude
    @NotBlank(message = "ログインIDを入力してください")
    private String loginId;

    /** パスワード */
    @ToString.Exclude
    @NotBlank(message = "パスワードを入力してください")
    private String password;

    /** ログインするイベントのID */
    @NotNull(message = "イベントIDを指定してください")
    private Long eventId;
}
```

#### response(大本:EventOverviewResponse)

```java
package org.example.app.dto.response.eventoverview;

import org.example.app.security.config.RoleType;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * イベント概要レスポンス
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "イベント概要レスポンス")
public class EventOverviewResponse {

    @Schema(description = "ロール（ADMIN か STUDENT）", example = "ADMIN", requiredMode = Schema.RequiredMode.REQUIRED)
    private RoleType role;

    @Schema(description = "イベント関連情報", requiredMode = Schema.RequiredMode.REQUIRED)
    private EventInfoDto eventInfo;

    @Schema(description = "イベント申請全体情報", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSummaryDto applicationSummary;

    @Schema(description = "申請情報管理", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationManagementDto applicationManagement;
}
```

#### response(EventInfoDto)

```java
package org.example.app.dto.response.eventoverview;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * イベント関連情報
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "イベント関連情報")
public class EventInfoDto {

    @Schema(description = "ブース総数", example = "34", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer boothCount;

    @Schema(description = "参加生徒数", example = "120", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer studentCount;

    @Schema(description = "承認済みの申請数（申請承認進捗の分子）", example = "1", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer approvedApplicationCount;

    @Schema(description = "申請の総数（申請承認進捗の分母）", example = "34", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer totalApplicationCount;
}
```

#### response(ApplicationSummaryDto)

```java
package org.example.app.dto.response.eventoverview;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * イベント申請全体情報
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "イベント申請全体情報")
public class ApplicationSummaryDto {

    @Schema(description = "予算申請合計（円）", example = "500000", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer budgetApplicationTotal;

    @Schema(description = "予算確定金額合計（円）", example = "420000", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer budgetConfirmedTotal;

    @Schema(description = "売上合計（円）", example = "800000", requiredMode = Schema.RequiredMode.REQUIRED)
    private Integer salesTotal;
}
```

#### response(ApplicationSettingDto)

```java
package org.example.app.dto.response.eventoverview;

import java.time.LocalDateTime;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * 申請種別ごとの申請情報
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "申請種別ごとの申請情報")
public class ApplicationSettingDto {

    @Schema(description = "申請名", example = "ブース申請", requiredMode = Schema.RequiredMode.REQUIRED)
    private String applicationName;

    @Schema(description = "締め切り日時（未設定の場合はnull）", example = "2026-10-31T23:59:00", requiredMode = Schema.RequiredMode.REQUIRED, nullable = true)
    private LocalDateTime deadlineAt;

    @Schema(description = "注意事項（未設定の場合はnull）", example = "レイアウト図を必ず添付してください。", requiredMode = Schema.RequiredMode.REQUIRED, nullable = true)
    private String remarks;

    @Schema(description = "更新日（未設定の場合はnull）", example = "2026-10-07T10:31:00", requiredMode = Schema.RequiredMode.REQUIRED, nullable = true)
    private LocalDateTime updatedAt;
}
```

#### response(ApplicationManagementDto)

```java
package org.example.app.dto.response.eventoverview;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

/**
 * 申請情報管理
 */
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@ToString
@Schema(description = "申請情報管理")
public class ApplicationManagementDto {

    @Schema(description = "ブース申請", example = """
        {"applicationName":"ブース申請","deadlineAt":"2026-10-31T23:59:00","remarks":"レイアウト図を必ず添付してください。","updatedAt":"2026-10-07T10:31:00"}""", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSettingDto boothApplication;

    @Schema(description = "売上申請", example = """
        {"applicationName":"売上申請","deadlineAt":"2026-11-05T23:59:00","remarks":"金種ごとの枚数を入力してください。","updatedAt":"2026-10-07T10:31:00"}""", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSettingDto salesApplication;

    @Schema(description = "備品申請", example = """
        {"applicationName":"備品申請","deadlineAt":"2026-11-01T23:59:00","remarks":"必要な備品と数量を入力してください。","updatedAt":"2026-10-07T10:31:00"}""", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSettingDto equipmentApplication;

    @Schema(description = "予算申請", example = """
        {"applicationName":"予算申請","deadlineAt":"2026-10-20T23:59:00","remarks":"見積書を添付してください。","updatedAt":"2026-10-07T10:31:00"}""", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSettingDto budgetApplication;

    @Schema(description = "予算使用申請", example = """
        {"applicationName":"予算使用申請","deadlineAt":"2026-11-10T23:59:00","remarks":"領収書の画像を添付してください。","updatedAt":"2026-10-07T10:31:00"}""", requiredMode = Schema.RequiredMode.REQUIRED)
    private ApplicationSettingDto budgetUsageApplication;
}
```

#### doc/template(共通エラー)

```java
package org.example.app.doc.template;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.example.app.exception.ErrorResponse;

import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.ExampleObject;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;

/**
 * 全APIで共通のエラーレスポンスのAPI仕様書定義
 * <p>
 * 各ApiDocのメソッドアノテーションに、1行で付与して使う。 例：{@code @CommonErrorApiDoc.Unauthorized}
 */
public class CommonErrorApiDoc {

    /** 400：@PathVariable・@RequestParam のバリデーションエラー */
    @Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @ApiResponse(responseCode = "400", description = "パラメータ不正", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "パラメータ不正", value = """
        {
          "errorCode": "INVALID_PARAMETER",
          "message": "リクエストパラメータが不正です"
        }
        """)))
    public @interface InvalidParameter {
    }

    /** 401：未ログイン・JWTの期限切れ・不正 */
    @Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @ApiResponse(responseCode = "401", description = "未ログイン・セッション切れ", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "未ログイン", value = """
        {
          "errorCode": "UNAUTHORIZED",
          "message": "ログインしていないか、有効期限が切れています。再度ログインしてください"
        }
        """)))
    public @interface Unauthorized {
    }

    /** 403：ロール不足・RoleRules未登録のURL */
    @Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @ApiResponse(responseCode = "403", description = "権限不足", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "権限不足", value = """
        {
          "errorCode": "FORBIDDEN",
          "message": "この操作を行う権限がありません"
        }
        """)))
    public @interface Forbidden {
    }

    /** 500：想定外のエラー */
    @Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @ApiResponse(responseCode = "500", description = "想定外のエラー", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "想定外のエラー", value = """
        {
          "errorCode": "INTERNAL_SERVER_ERROR",
          "message": "サーバー内部でエラーが発生しました"
        }
        """)))
    public @interface InternalServerError {
    }

    /** 401・403・500をまとめたもの（ログイン必須の管理者向けAPIで共通） */
    @Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @ApiResponse(responseCode = "401", description = "未ログイン・セッション切れ", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "未ログイン", value = """
        {
          "errorCode": "UNAUTHORIZED",
          "message": "ログインしていないか、有効期限が切れています。再度ログインしてください"
        }
        """)))
    @ApiResponse(responseCode = "403", description = "権限不足", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "権限不足", value = """
        {
          "errorCode": "FORBIDDEN",
          "message": "この操作を行う権限がありません"
        }
        """)))
    @ApiResponse(responseCode = "500", description = "想定外のエラー", content = @Content(mediaType = "application/json", schema = @Schema(implementation = ErrorResponse.class), examples = @ExampleObject(name = "想定外のエラー", value = """
        {
          "errorCode": "INTERNAL_SERVER_ERROR",
          "message": "サーバー内部でエラーが発生しました"
        }
        """)))
    public @interface Authenticated {
    }
}
```

</details>

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

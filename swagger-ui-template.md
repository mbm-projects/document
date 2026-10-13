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

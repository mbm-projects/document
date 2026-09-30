## 命名規則

| 対象要素 | 表記法 | 命名例 |
| --- | --- | --- |
| **クラス (Class)** | アッパーキャメルケース | `UserService`, `OrderController`, `DatabaseManager` |
| **インターフェース (Interface)** | アッパーキャメルケース | `UserMapper`, `Runnable`, `Serializable` |
| **メソッド (Method)** | ローワーキャメルケース | `getUserById()`, `saveOrder()`, `isEmpty()` |
| **変数 / フィールド (Variable / Field)** | ローワーキャメルケース | `userId`, `totalPrice`, `createdAt` |
| **定数 (Constant)** | 大文字スネークケース | `MAX_RETRY_COUNT`, `DEFAULT_PAGE_SIZE` |
| **パッケージ (Package)** | すべて小文字 | `com.example.project.service` |
| **型パラメータ (Generics)** | 大文字1文字 | `T`, `E`, `K`, `V` |

## コーディング規約

| 分類 | ルール | 具体例・補足 |
| --- | --- | --- |
| **インデント** | スペース4つで統一する（タブ文字は禁止） | — |
| **改行コード** | `LF` に統一する | Windows環境でも `CRLF` は使用しない |
| **1行の文字数** | 最大120文字とする | 超える場合はカンマや演算子の後で改行する |
| **波括弧の位置** | 開始括弧 `{` は行末に置く（K&Rスタイル）。クラス・メソッド宣言、`if` / `for` / `while` などすべての構文に適用する | `if (condition) {`<br>`    // 処理`<br>`}` |
| **空白** | キーワードの後、および演算子の前後に半角スペースを1つ入れる | OK: `if (a == b)`<br>NG: `if(a==b)` |
| **ファイル末尾** | ファイルの最後は必ず改行で終える | 最後の `}` の後に改行を1つ入れる |
| **インポート** | ワイルドカード（`*`）によるインポートは禁止し、クラスを個別に指定する | NG: `import java.util.*;` |
| **アノテーション** | クラス・メソッドに付与するアノテーションは、宣言とは別の行に記述する | `@Override`<br>`public void run() { ... }` |
| **例外処理** | catchした例外を何もせずに握り潰すことを禁止する | 業務例外は独自例外（`BusinessException` の継承クラス）に変換してthrowするか、ログを出力する |
| **Lombok** | 必要最低限のアノテーションのみ使用する | `@Data` は使用可。ただしsetterやコンストラクタが不要な場合は `@Getter` など必要なものだけを付与する |
| **アクセス修飾子** | フィールドは原則 `private` とし、外部から参照する場合はgetter経由とする | `private String userName;` |
| **コメント** | クラス・メソッドには必ずJavadocコメントを記述し、処理内にも適宜コメントを残す | `/** ユーザー情報を取得する */`<br>`// 入力値をチェックする` |
| **未使用コード** | 未使用のimport・変数・デッドコードを残さない | IDEの警告（黄色の下線）はその都度解消する |
| **メソッドの行数** | 1メソッドは50行以内を目安とし、超える場合は意味のある単位でprivateメソッドに分割する | 分割するとかえって可読性が下がるなど、合理的な理由がある場合は超えてもよい |

## 使用ライブラリ

- Spring Boot (spring-boot-starter-parent 3.5.6)
- Spring Web (spring-boot-starter-web)
- Spring Security (spring-boot-starter-security)
- Spring JDBC (spring-boot-starter-jdbc)
- MyBatis (mybatis-spring-boot-starter)
- PostgreSQL Driver (postgresql)
- Lombok
- JWT (jjwt-api / jjwt-impl / jjwt-jackson)
- Spring Boot DevTools
- Spring Boot Test (spring-boot-starter-test)
- MyBatis Test (mybatis-spring-boot-starter-test)
- springdoc-openapi

## HTTPSTATUS

| ステータスコード | 名称 | 用途 | 発生場所（目安） | Spring Bootでの呼び出し例 |
| --- | --- | --- | --- | --- |
| 200 OK | OK | 取得・更新・処理成功時の標準レスポンス | GET / PUT / PATCH 成功時 | `ResponseEntity.ok(responseDto);` / `ResponseEntity.status(HttpStatus.OK).body(responseDto);` |
| 201 Created | Created | 新規リソース作成成功時 | POST（登録処理）成功時 | `ResponseEntity.status(HttpStatus.CREATED).body(responseDto);` |
| 204 No Content | No Content | 処理成功したがレスポンスボディが不要な場合 | DELETE 成功時、更新のみで返却データなし | `ResponseEntity.noContent().build();` |
| 400 Bad Request | Bad Request | リクエスト自体の形式不備（バリデーションエラー含む） | @Valid でのバリデーション失敗時 | `ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errorResponse);` |
| 401 Unauthorized | Unauthorized | 未認証（ログインしていない、トークン不正・期限切れ） | JWT検証失敗、未ログインアクセス | `ResponseEntity.status(HttpStatus.UNAUTHORIZED).body(errorResponse);` |
| 403 Forbidden | Forbidden | 認証済みだが権限不足でアクセス拒否 | ロール・権限チェック失敗時 | `ResponseEntity.status(HttpStatus.FORBIDDEN).body(errorResponse);` |
| 404 Not Found | Not Found | 指定したリソースが存在しない | IDで検索したが対象データが無い場合 | `ResponseEntity.status(HttpStatus.NOT_FOUND).body(errorResponse);` |
| 409 Conflict | Conflict | リソースの状態が矛盾・重複している | 重複登録（一意制約違反）、排他制御エラー | `ResponseEntity.status(HttpStatus.CONFLICT).body(errorResponse);` |
| 422 Unprocessable Entity | Unprocessable Entity | 形式は正しいが業務ルール上処理できない場合（400と使い分ける場合のみ採用） | 業務ロジック上のエラー（在庫不足等） | `ResponseEntity.status(HttpStatus.UNPROCESSABLE_ENTITY).body(errorResponse);` |
| 500 Internal Server Error | Internal Server Error | 想定外のサーバーエラー（バグ・DB接続断など） | catch されなかった例外（Exceptionクラス） | `ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(errorResponse);` |

## 環境変数

| カテゴリ | 環境変数名 | 説明 | 例（開発環境） | 管理方法 |
| --- | --- | --- | --- | --- |
| Front | FRONT_HOST | フロントエンドのホスト名（ドメイン部分のみ） | localhost | application-{profile}.yml |
| Front | FRONT_PORT | フロントエンドのポート番号（本番で省略可） | 3000 | application-{profile}.yml |
| Front | FRONT_REDIRECT_URL | 認証後などのリダイレクト先URL | http://localhost:3000/login/callback | application-{profile}.yml |
| DB | DB_HOST | PostgreSQL接続ホスト | localhost | .env（git管理外） |
| DB | DB_PORT | PostgreSQL接続ポート | 5432 | .env（git管理外） |
| DB | DB_NAME | データベース名 | graduation_project_db | .env（git管理外） |
| DB | DB_USERNAME | DB接続ユーザー名 | app_user | .env（git管理外） |
| DB | DB_PASSWORD | DB接続パスワード | ******** | .env（git管理外・機密） |
| JWT | JWT_SECRET_KEY | JWT署名用シークレットキー | ********（32文字以上のランダム文字列） | .env（git管理外・機密） |
| JWT | JWT_EXPIRATION | トークンの有効期限（ミリ秒 or 秒） | 3600000（1時間） | application-{profile}.yml |
| JWT | JWT_ISSUER | トークン発行者（iss claim） | graduation-project-api | application-{profile}.yml |
| Cookie | COOKIE_SECURE | Cookieのsecure属性有無（本番はtrue必須） | false（開発） / true（本番） | application-{profile}.yml |
| Cookie | COOKIE_SAME_SITE | CookieのSameSite属性（CSRF対策） | Lax（開発） / None（本番・別ドメイン時） | application-{profile}.yml |
| Cookie | COOKIE_DOMAIN | Cookieの発行対象ドメイン | localhost | application-{profile}.yml |
| Server | SERVER_PORT | Spring Bootアプリ（バックエンド）の起動ポート | 8080 | application-{profile}.yml |
| Profile | SPRING_PROFILES_ACTIVE | 使用するプロファイル（環境切り替え） | local / dev / prod | 起動時オプション or .env |
|  |  |  |  |  |

## application.properties

| ファイル | 設定キー | 説明 |
| --- | --- | --- |
| application.yml | spring.application.name | アプリケーション名 |
| application.yml | spring.profiles.active | 有効にするプロファイル（local/dev/prod） |
| application.yml | db.url | DB接続用URL（host, port, db名を組み合わせ） |
| application.yml | db.username | DB接続ユーザー名 |
| application.yml | db.password | DB接続パスワード |
| application.yml | db.driver-class-name | JDBCドライバークラス（PostgreSQL） |
| application.yml | spring.jackson.default-property-inclusion | レスポンスJSONでnullフィールドを含めるかどうか |
| application.yml | server.port | バックエンドの起動ポート |
| application.yml | mybatis.configuration.map-underscore-to-camel-case | DBカラム名(snake_case)とJavaフィールド名(camelCase)の自動変換 |
| application.yml | mybatis.mapper-locations | XMLマッパーファイルの配置場所 |
| application.yml | app.front.host | フロントエンドのホスト名 |
| application.yml | app.front.port | フロントエンドのポート番号 |
| application.yml | app.front.url | フロントエンドの完全URL（host+portをまとめたベースURL） |
| application.yml | app.front.redirect-url | 認証後などのリダイレクト先URL |
| application.yml | app.jwt.secret-key | JWT署名用シークレットキー |
| application.yml | app.jwt.expiration | JWTトークンの有効期限 |
| application.yml | app.jwt.issuer | JWTの発行者情報（iss claim） |
| application.yml | app.cookie.secure | CookieのSecure属性（HTTPS限定送信するか） |
| application.yml | app.cookie.same-site | CookieのSameSite属性（CSRF対策、Lax/None/Strict） |
| application.yml | app.cookie.domain | Cookie発行対象のドメイン |
| application.yml | springdoc.api-docs.path | OpenAPI仕様書（JSON）のパス |
| application.yml | springdoc.swagger-ui.path | Swagger UIのパス |
| application.yml | springdoc.swagger-ui.operations-sorter | Swagger UI内のAPI表示順（method順など） |
| application-local.yml | logging.level.root | ローカル環境の全体ログレベル（INFO） |
| application-local.yml | logging.level.com.example.project | 自プロジェクトパッケージのログレベル（DEBUG） |
| application-local.yml | springdoc.swagger-ui.enabled | ローカルでSwagger UIを有効化 |
| application-prod.yml | logging.level.root | 本番環境の全体ログレベル（WARN） |
| application-prod.yml | springdoc.swagger-ui.enabled | 本番でSwagger UIを無効化（セキュリティ対策） |
| application-prod.yml | springdoc.api-docs.enabled | 本番でAPI仕様書のエンドポイント自体を無効化 |
| CorsConfig（config配置） | allowedOrigins | CORSで許可するオリジン（app.front.url を使用） |
| CorsConfig（config配置） | allowedMethods | 許可するHTTPメソッド（GET, POST, PUT, DELETE, PATCH, OPTIONS） |
| CorsConfig（config配置） | allowedHeaders | 許可するリクエストヘッダー |
| CorsConfig（config配置） | allowCredentials | Cookie送信を許可するか（HttpOnly Cookie運用時はtrue必須） |
| CorsConfig（config配置） | maxAge | プリフライトリクエストのキャッシュ時間（秒） |

## RestfullApi命名規則

| 対象 | ルール | 例 |
| --- | --- | --- |
| URL全体 | 小文字・ケバブケース | /api/user-profiles |
| リソース名 | 複数形の名詞を使う（動詞は使わない） | /api/users （× /api/getUser） |
| 単一リソース | リソース名 + パスパラメータ（ID） | /api/users/{userId} |
| ネスト関係 | 親リソース/{id}/子リソース | /api/users/{userId}/orders |
| 検索・絞り込み | クエリパラメータで表現 | /api/users?status=active&page=1 |
| 取得（一覧） | GET + 複数形リソース | GET /api/users |
| 取得（単体） | GET + 複数形リソース/{id} | GET /api/users/{userId} |
| 作成 | POST + 複数形リソース | POST /api/users |
| 更新（全体） | PUT + 複数形リソース/{id} | PUT /api/users/{userId} |
| 更新（一部） | PATCH + 複数形リソース/{id} | PATCH /api/users/{userId} |
| 削除 | DELETE + 複数形リソース/{id} | DELETE /api/users/{userId} |
| 特殊操作 | 動詞をサブリソース化して表現 | POST /api/users/{userId}/activate |

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

## パッケージ構成

| パッケージ / ディレクトリ | 役割・説明 |
| --- | --- |
| **`controller/api`** | 外部からのHTTPリクエストを受け付け、レスポンスを返却する境界層。 |
| **`service`** | 業務ロジック（ビジネスロジック）を実装する層。 |
| **`repository`** | データアクセス層。Mapperを呼び出し、永続化（DB）に関する処理を隠蔽する層。 |
| **`mapper`** | SQLとJavaオブジェクトのマッピングを行う層（MyBatisなど）。 |
| **`entity`** | DBのテーブルと1対1で対応するデータモデル。 |
| **`constant`** | DBのコード定義（数値と日本語の変換）などを管理するEnum配置層。 |
| **`dto`** | リクエスト（request）、レスポンス（response）、entity以外の形でのDBアクセスオブジェクト（db）などのJavaオブジェクトを扱う層。 |
| **`exception`** | アプリケーション固有の例外クラスや、グローバルな例外ハンドラー（@ControllerAdviceなど）を管理する層。 |
| **`security`** | 認証・認可、ログイン処理、アクセス制御に関する設定や処理を管理する層。 |
| **`util`** | 日付操作や文字列加工など、プロジェクト全体で共通利用する汎用的な関数群。 |
| **`config`** | 各ライブラリ（DB接続やセキュリティ、その他外部連携など）の設定ファイルを扱う層。 |
| **`doc`** | springdoc-openapiで扱う仕様書や、API仕様書のURLなどをまとめたファイルを配置する層。 |

---

## パッケージ詳細

#### 用語解説

- 単数：　引き数(引数の変数)・戻り値が一個という意味(引数・戻り値の欄に配列・コレクションがあればList<string>のような形で受け取ってよい)
    - method(引数1))
- 複数：引数・戻り値が複数個でいいという意味
    - method(引数1,引数2))

### controller/api

外部（フロントエンド）からのエンドポイントを提供し、リクエストのバリデーション（検証）とレスポンスの返却を行います。ロジックは記述せず、Serviceを呼び出す。

- **引数**
    - `dto/request` パッケージのオブジェクト（単数）
    - `@PathVariable` や `@RequestParam` で受け取るプリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等（単数）
    - `void`
- **戻り値**
    - `ResponseEntity<T>` 形式でラップした以下のもの
        - `dto/response` パッケージのオブジェクト（単数）
        - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等（単数）
        - `void`
    
- **制約**
    - バリデーションは`dto/request`パッケージが引数の場合はdtoにその他の引数の場合は`controller/api`内に記述
    - `entity`パッケージをそのまま戻り値・引数として使用しない
    - `dto/db` は戻り値・引数では使用しない(`dto/request`,`dto/response` に詰めなおす)
    

---

### service

アプリケーションの核となるビジネスロジックを記述します。トランザクション管理（`@Transactional`）はこの層で行います。

- **引数**
    - `dto/request` パッケージオブジェクト
    - `dto/db`  パッケージオブジェクト（複数可）
    - その他`dto/`直下のパッケージのオブジェクト（複数可）
    - `entity` パッケージのオブジェクト（複数可）
    - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等（複数可）
    - その他オブジェクト(複数可)
    - `void`
- **戻り値**
    - `dto/response`  パッケージオブジェクト
    - `dto/db`パッケージのオブジェクト（複数可,単数推奨）
    - その他`dto/`直下のパッケージのオブジェクト（複数可,単数推奨）
    - `entity`パッケージのオブジェクト(複数可,単数推奨)
    - プリミティブ型、String型、配列・コレクション等（複数可）
    - `void`
    - その他オブジェクト(複数可)
- **制約**
    - `@Transactional` 処理は基本この階層で行う
- **その他**
    - 単純な汎用関数は`util` に記載するので注意
    - 外部から呼ばれる場合はpublic,関数内のコードが長くなる場合はprivateで関数化して呼び出す。
    - serviceパッケージは制約緩いが学生の開発のためある程度の緩さは許容するものとする

---

### repository

データアクセス。Serviceから呼び出され、内部でMapperを呼び出してDB操作を行います。

- **引数**
    - `dto/db` パッケージのオブジェクト（単数）
    - `entity` パッケージのオブジェクト（単数）
    - プリミティブ型、String型、配列・コレクション・その他DBカラムに関する標準ライブラリ型オブジェクト等（複数可）
    - `void`
- **戻り値**
    - `entity` パッケージのオブジェクト（単数）
    - `dto/db` パッケージのオブジェクト（単数）
    - プリミティブ型(int・longの件数など)、String型、配列・コレクション・その他DBカラムに関する標準ライブラリ型オブジェクト等（複数可）
- **制約**
    - 引数として`dto/request`を渡しそうになるが受け取ってはならない(汎用性を持たせるため)
- その他
    - insert（新規作成処理）の際自動採番のidの値を元のオブジェクトに詰めてもよい
    - 引数として値のバリエーション(if文等)行う場合は記述せず、行う場合はserviceやutilなどで行う
    - `repository`クラスは`service`クラスからのみ呼び出される
    - `repository`クラスは`mapper`クラスからのみを呼びだす
    - 引数`dto/db` パッケージ・`entity` パッケージの使い分け
        - `entity` パッケージ
            - entityのフィールドをすべて利用する場合(例外として自動採番のid・デフォルト値が戻り値で使用する場合は空でよい)
        - `dto/db` パッケージ
            - 上記の`entity` パッケージの利用条件を満たさない場合
        - 例: INSERTで一部カラムのみ指定 → 引数はdto/db、全カラム取得 → 戻り値はentity
    - 戻り値`dto/db` パッケージ・`entity` パッケージの使い分け
        - `entity` パッケージ
            - entityのフィールドをすべて利用する場合
        - `dto/db` パッケージ
            - entityのフィールドをすべてを利用しない場合
        

---

### mapper

MyBatisのマッピングインターフェースです。基本はメソッドに `@Select` や `@Insert` などのアノテーションを付与して直接SQLを記述する。複雑なSQLになる場合のみXMLファイルを使用する。

- **引数**
    - `dto/db` オブジェクト (複数可,単数)
    - `entity` オブジェクト (複数可,単数推奨)
    - プリミティブ型((int・longの件数など))、String型、配列・コレクション・その他DBカラムに関する標準ライブラリ型オブジェクト等（複数可）
    - `void`
    
- **戻り値**
    - `entity` パッケージのオブジェクト（単数）
    - `dto/db` パッケージのオブジェクト（単数）
    - プリミティブ型((int・longの件数など))、String型、配列・コレクション・その他DBカラムに関する標準ライブラリ型オブジェクト等（複数可：複数可だが特に3以上の場合はentityの利用及びdtoの作成推奨 ）
- **制約**
    - アノテーションで書けない複雑なSQLでXMLファイルを使用する場合は、Javaファイル側（`src/main/java/...`）ではなく、**`src/main/resources` 配下にmapperと同じパッケージ階層のフォルダを作成して配置すること**。
    - @Paramアノテーションを利用する
    - `mapper`クラスは`repository`クラスからのみ呼び出される

---

### entity

データベースのテーブル構造と1対1でマッピングされるオブジェクト(Viewも含む)。

- **フィールド**
    - プリミティブ型、String型、配列・コレクション等・その他DBカラムに関する標準ライブラリ型オブジェクト等
- **引数**
    - フィールドに関するもの(setter,コンストラクタのみ)
- **戻り値**
    - フィールドに関するもの(getterのみ)
- **制約**
    - テーブルの全カラムをフィールドとして持ち、データ型はDBの型と一致させる。
    - レスポンスのオブジェクト構造の都合による加工用フィールドを追加してはならない(`dto/db`パッケージを作成し欲しいフィールドを追加する)。
    - テーブル定義でnullを許容するフィールドはプリミティブ型ではなく対応するラッパークラスを使用する
    - メソッドはコンストラクタ・setter・getterのみ。フィールドに関する値以外を引数・戻り値として扱ってはならない。
    - Viewに関するentityはクラス名にViewをつける(ViewはデータベースのViewを指す)
    - `entity`パッケージ内でのパッケージ訳はviewで独立させるのではなく、役割が近いクラスごとでパッケージ化を行う(例：userパッケージ以下 User.java,UserInfoView.java)
- その他
    - 引数として値のバリエーション(if文等)行う場合は記述せず、行う場合はserviceやutilなどで行う

---

### dto

リクエスト(request)、レスポンス(response)、entity以外の形でのDBアクセスオブジェクト(db)などのJavaオブジェクトを扱います。 `dto/request`・`dto/response`・`dto/db` ・その他(`dto/`直下)(dto/直下に直接配置や役割ごとにさらにパッケージを追加する)のサブパッケージに分類して配置する。

#### dto/request

Controllerが受け取るリクエスト内容を表現するオブジェクト。

- **フィールド**
    - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等
    - `dto/request/リクエストパッケージ名`直下のオブジェクト
- **引数**
    - 開発者は明示的に呼び出すことはしない
    - Jacksonなどのフレームワークが内部的に使用する(フィールドに関するもの(setter,コンストラクタのみ))
- **戻り値**
    - フィールドに関するもの(getterのみ)
- **制約**
    - バリデーションアノテーション（`@NotNull`、`@Size`等）をフィールドに付与する。
    - 基本getterのみを呼び出して利用し、コンストラクタ・setterは開発では利用しない(例外的にapiの内部処理等を変更する場合は利用可)
    - requestで`{ user: {...} }` で受け取りたい場合
        - `entity` パッケージは利用しない
        - `entity` パッケージを利用せずdtoを作成し利用する場合
            - 複数個所で使用されるdtoの場合
                - その他(`dto/`直下)にdtoを作成し利用する
            - そのrequestのみでしか使用しないdtoの場合
                - `dto/request/` パッケージにそのリクエスト用のパッケージを作成しその中にdtoを作成する(例：signinrequestパッケージを作成しその中にSigninRequest.javaとSinginStudentInfoDto.java)
- **その他**
    - `dto`パッケージオブジェクトを利用する場合はフィールド名をアノテーションを使用して変更する。
    - `dto/db` ,`dto/response` パッケージと同じ構造を利用したい場合でも`dto/db` パッケージは使用せず、新しいその他`dtoパッケージ`に詰めなおして利用する。(他のの処理との結合度を低くするため)
    

#### dto/response

Controllerが返却するレスポンス内容を表現するオブジェクト。

- **フィールド**
    - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等
    - `dto/response/レスポンスパッケージ名`直下のオブジェクト
- **引数**
    - フィールドに関するもの(setter,コンストラクタのみ)
- **戻り値**
    - フィールドに関するもの(getterのみ)
- **制約**
    - setter,getter.コンストラクタのみを呼び出して利用する。
    - responseで`{ user: {...} }` で返したい場合
        - `entity` パッケージは利用しない
        - `entity` パッケージを利用せずdtoを作成し利用する場合
            - 複数個所で使用されるdtoの場合
                - その他(`dto/`直下)にdtoを作成し利用する
            - そのresponseのみでしか使用しないdtoの場合
                - `dto/response/` パッケージにそのレスポンス用のパッケージを作成しその中にdtoを作成する(例：userinforesponse/パッケージを作成しその中にUserInfoResponse.javaとClassInfoDto.java)
    - エラーレスポンスオブジェクトは`exception`パッケージのカスタム例外をスローして対応する
    - `dto/db` 同じ構造(dbの結果をそのまま返す)は`of` メソッド(`dto/db` の内容を詰め替える)を用意する
        - コード例(これを呼び出す)
            
            public static UserResponseDto of(UserDbDto userDbDto) {
            return new UserResponseDto(
            entity.getId(),
            entity.getName(),
            entity.getEmail(),
            entity.getBirthday().format(DateTimeFormatter.ofPattern("yyyy/MM/dd")),
            Period.between(entity.getBirthday(), LocalDate.now()).getYears()
            );
            }
            
        - リストの場合
        
        `List<UserDetailViewDto> dtoList = userList.stream()
        .map(UserDetailViewDto::of) // ★ これだけで全件変換できる！
        .toList();`
        
- **その他**
    - `dto`パッケージオブジェクトを利用する場合はフィールド名をアノテーションを使用して変更する。
    - `dto/db` ,`dto/request` パッケージと同じ構造を利用したい場合でも`dto/db` パッケージは使用せず、新しいに詰めなおして利用する。(他の処理との結合度を低くするため)

#### dto/db

Repository・Mapperで扱う、entityでは表現できないDBアクセス用オブジェクト（複数テーブルのJOIN結果、集計結果、検索条件等）。

- **フィールド**
    - `entity`パッケージのオブジェクト
    - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等
    - `dto/db/dbパッケージ名`直下のオブジェクト
- **引数**
    - フィールドに関するもの(setter,コンストラクタのみ)
- **戻り値**
    - フィールドに関するもの(getterのみ)
- **制約**
    - setter,getter.コンストラクタのみを呼び出して利用する。
    - `{ user: {...} }` のフィードを使用したい場合
        - `entity` パッケージを利用する
        - `entity` パッケージを利用せずdtoを作成し利用する場合
            - 複数個所で使用されるdb関係のdtoの場合
                - その他(`dto/db`直下)にdb関係のdtoを作成し利用する
            - その`dto/db`のみでしか使用しないdtoの場合
                - `dto/db/` パッケージにその`dto/db`用のパッケージを作成しその中にdtoを作成する(例：userdbdto/パッケージを作成しその中にUserDbDto.javaとUserInfoDbDto.java)
- **その他**
    - 他の`dto/` パッケージと同じ構造を利用したい場合でも他の`dto/` パッケージは使用せず、新しく`dto/db`パッケージを作成して利用する。(他の処理の結合度を低くするため)

#### dto/直下

 `dto/request`・`dto/response`・`dto/db` に含まれないその他のdto

- **フィード**
    - `entity`パッケージのオブジェクト(`dto/request`・`dto/response`内で利用される場合は利用不可)
    - プリミティブ型、String型、配列・コレクション・標準ライブラリ型オブジェクト等
    - その他オブジェクト
    - その他(`dto/` 直下)パッケージのオブジェクト
- **引数**
    - フィールドに関するもの(setter,コンストラクタのみ)
- **戻り値**
    - フィールドに関するもの(getterのみ)
- **制約**
    - setter,getter.コンストラクタのみを呼び出して利用する。
    - `{ user: {...} }` のフィードを使用したい場合
        - `entity`パッケージのオブジェクト(`dto/request`・`dto/response`内で利用される場合は利用不可)
        - `entity` パッケージを利用せずdtoを作成し利用する場合
            - 複数個所で使用されるdtoの場合
                - その他(`dto/`直下)にdtoを作成し利用する
            - その`dto/`のみでしか使用しないdtoの場合
                - その他(`dto/`直下)パッケージにそのdto用のパッケージを作成しその中にdtoを作成する(例：userInfodto/パッケージを作成しその中にUserInfoDto.javaとUser○○Dto.java)
    
- **その他**
    - `dto/db` ,`dto/request` ,`dto/response`パッケージと同じ構造を利用したい場合でも`dto/db` パッケージは使用せず、新しいその他`dtoパッケージ`に詰めなおして利用する。(他の処理との結合度を低くするため)

---

### constant

DBの区分値（ステータスコードなど）と日本語ラベルを紐付けるEnum（列挙型）などを配置します。

- **フィールド**
    
    enum型の以下の値を内包する
    
    - code
        - int型
        - コードを表す
    - label
        - String 型
        - コードに紐づく日本語名を表す
- **制約**
    - 定義する値は必ず「数値（コード）」と「日本語（ラベル）」をセットで管理する。
    - 数値からEnumを逆引きする静的メソッド（`fromCode(int code)`）を用意する。
- **実装する関数**
    - getCode()
        
        codeの値(int)を返す
        
        - 引数
            - なし
        - 戻り値
            - code(int)
    - getLabel()
        
        codeからlabelを取得する
        
        - 引数
            - なし
        - 戻り値
            - label(String)
    - fromCode()
        
        codeの値からenumを逆引きする関数(db保存用)。
        
        存在しない場合は例外を投げる
        
        - 引数
            - code(int)
        - 戻り値
            - 定義したenum型
        

---

### exception

カスタム例外クラスと、アプリケーション全体で発生した例外をキャッチして共通の型でエラーレスポンスを返すハンドラー(`@RestControllerAdvice`)から成る。

- **パッケージ構成**
    - `exception` 直下: `BusinessException`・`ErrorResponse`・`GlobalExceptionHandler`
    - `exception/validation`: `ValidationException`・`ValidationErrorResponse` と、その継承クラス
    - `exception/{役割名}`: 役割ごとの業務例外(例：`user/`,`auth/`)
- **制約**
    - すべての業務例外は `BusinessException` を継承し、コンストラクタで `HttpStatus`・`errorCode`・`message` をセットする。
        - `GlobalExceptionHandler` は業務例外の場合は `BusinessException` を1回だけ `@ExceptionHandler` し、例外が保持する値を取り出して共通の `ErrorResponse` を組み立て、レスポンスとして返す。ただし `BusinessException` を継承した `ValidationException` は項目別のエラーを持つため、別途 `@ExceptionHandler` で処理し `ValidationErrorResponse` を返す。その他の例外は一つずつ記述を行い、`ErrorResponse` を返す。
    - 各エクセプションは役割ごとにサブパッケージ化を行う。例：`user/`,`auth/`
    - セキュリティ関係のhandlerは業務例外ではないため`security` パッケージ内に配置する
    - バリデーションチェックのエラーも対応する
    - ログ出力は `GlobalExceptionHandler` で一元的に行う(ログルールは下記参照)
    - errorCodeは大文字スネークケース(例：`USER_NOT_FOUND`)で統一する
- **ログ出力ルール**
    - **出力場所**
        - 例外のログは `GlobalExceptionHandler` の各 `@ExceptionHandler` 内で必ず出力する(ログを出さずにレスポンスだけ返してはならない)
        - 二重出力を防ぐため、例外をthrowする側(controller・service・repositoryなど)では同じ例外のログを出力しない
        - `security/handler` 配下のハンドラー(認証・認可エラー)も本ルールに従いログを出力する
    - **ログレベルとスタックトレース**
        - 業務例外(`BusinessException`)
            - `WARN`
            - `errorCode`・`message`・HTTPステータスを出力する(スタックトレースは出力しない)
        - バリデーションエラー・リクエスト形式不正・存在しないURLへのアクセスなど、リクエスト起因のエラー
            - `WARN`
            - エラー内容が分かるメッセージを出力する(スタックトレースは出力しない)
        - 想定外の例外(`Exception`)
            - `ERROR`
            - スタックトレースを含めて出力する(例外オブジェクトをログの最後の引数に渡す)
    - **実装方法**
        - ロガーはLombokの `@Slf4j` を利用する
        - ログメッセージは日本語で記述し、値の埋め込みはプレースホルダ(`{}`)を利用する(文字列連結は禁止)
    - **出力してはいけない情報**
        - パスワード・トークン(JWT等)・Cookieの値・個人情報(メールアドレス、氏名など)はログに出力しない
        - バリデーションエラーの入力値(rejected value)はログに出力しない(項目名とメッセージのみ出力する)
    - **レスポンスとの関係**
        - 想定外の例外(500)はスタックトレースや例外メッセージなどの内部情報をレスポンスに含めず、固定メッセージを返す(詳細はログでのみ確認する)
- **各Exception**クラス
    - **フィールド**
        - なし(親クラスのフィールドを利用)
    - **引数**
        - errorCode や message の組み立てに必要な値(プリミティブ型、String型、entity/dtoパッケージのオブジェクト等)
    - **戻り値**
        - なし
    - **制約**
        - `BusinessException` を継承する
        - コンストラクタはsuper()を呼びだす
        - サブパッケージ内に配置する
        - `message` にはログに出力してもよい情報のみ含める(パスワード等の機密情報を含めない)
- **ErrorResponseクラス**
    - **フィールド**
        - errorCode
            - String
            - final
            - private
        - message
            - String
            - final
            - private
    - **引数**
        - フィールドに関するもの(コンストラクタのみ)
    - **戻り値**
        - フィールドに関するもの(getterのみ)
    - **制約**
        - `exception`直下に配置する
        - Lombokのgetterのみを利用する
        - 継承されることを想定し、フィールドは `private final`、コンストラクタは `public` とする
- **BusinessException**
    - **フィールド**
        - status
            - org.springframework.http.HttpStatus
            - final
            - private
        - errorCode
            - String
            - final
            - private
    - **引数**
        - コンストラクタで以下のものを受け取る
            - status
                - org.springframework.http.HttpStatus
            - errorCode
                - String
            - message
                - String
    - **戻り値**
        - ナシ
    - **制約**
        - Lombokを利用する(Getterのみ)
        - `RuntimeException` を継承する
        - コンストラクタのアクセス修飾子は`protected`
- **バリデーション(`exception/validation`)**
    - バリデーションエラーは項目別のエラーを返す特殊なエラーとして、`exception/validation` パッケージに独立して配置する
    - **ValidationErrorResponseクラス**
        - **フィールド**
            - errors
                - Map<String, String>
                - final
                - private
                - key は項目名、value はエラーメッセージ
        - **引数**
            - errorCode・message・errors を受け取るコンストラクタのみ
        - **戻り値**
            - フィールドに関するもの(getterのみ)
        - **制約**
            - `ErrorResponse` を継承する
            - Lombokのgetterのみを利用する
            - コンストラクタで `super()` を呼び出す
    - **ValidationExceptionクラス(継承用)**
        - **フィールド**
            - errors
                - Map<String, String>
                - final
                - private
                - key は項目名、value はエラーメッセージ
        - **引数**
            - コンストラクタで以下のものを受け取る
                - status(HttpStatus)
                - errorCode(String)
                - message(String)
                - errors(Map<String, String>)
        - **戻り値**
            - なし
        - **制約**
            - `BusinessException` を継承する
            - Lombokのgetterのみを利用する
            - コンストラクタのアクセス修飾子は `protected`(継承して利用する)
            - 項目単位でエラーを返したい業務エラー(重複登録、項目間の整合性エラーなど)は、これを継承した例外をthrowする
            - 継承先は `exception/validation` または役割ごとのサブパッケージに配置する
            - `errors` の key はリクエストのキー名(JSONのキー名)とする
    - **ハンドリング**
        - ハンドラーは `GlobalExceptionHandler` に記述する(バリデーション専用のハンドラークラスは作らない)
        - `MethodArgumentNotValidException`(`@Valid`)と `HandlerMethodValidationException`(`@PathVariable`・`@RequestParam`)をそれぞれ `@ExceptionHandler` で処理する
        - `ValidationException` を1回だけ `@ExceptionHandler` で処理する
        - 戻り値は `ResponseEntity<ValidationErrorResponse>` とする
    - **ステータス・エラーコード**
        - `@Valid` 等のバリデーションアノテーションによるエラーは、すべて `400 BAD_REQUEST`・errorCode `VALIDATION_ERROR` で返す(`422` は使用しない)
        - `message` は固定文言(`入力内容に誤りがあります`)とする
        - `ValidationException` 由来のエラーは、例外が保持する `status`・`errorCode`・`message` を利用する
    - **errorsの設定ルール**
        - key はリクエストのキー名(JSONのキー名)とする。snake_case変換している場合は変換後の名前にする
        - `@PathVariable`・`@RequestParam` の場合の key はパラメータ名とする
        - 同じ項目に複数のエラーがある場合は、最初の1件のみ設定する
        - アノテーションの `message` 属性に日本語のメッセージを必ず指定する(デフォルトメッセージは使用しない)
    - **バリデーションの記述場所**
        - `dto/request` を引数にする場合は、dtoのフィールドにバリデーションアノテーションを付与し、controllerの引数に `@Valid` を付ける
        - `@PathVariable`・`@RequestParam` の場合は、controllerの引数に直接アノテーションを付与する
        - controllerのクラスには `@Validated` を付けない(付けると `ConstraintViolationException` が発生し、上記のハンドラーで拾えなくなるため)
    - **ログ出力**
        - `WARN` で出力し、スタックトレースは出力しない
        - 入力された値(rejected value)はログに出力しない(項目名とメッセージのみ出力する)
    - **リクエスト形式不正(JSON不正・型不一致)**
        - `HttpMessageNotReadableException` はバリデーションエラーとは区別し、`400 BAD_REQUEST`・errorCode `INVALID_REQUEST_BODY`・固定メッセージ(`ErrorResponse`)で返す
        - ログは `WARN` で出力し、スタックトレースは出力しない
- **GlobalExceptionHandler**
    - **引数**
        - 各例外に関するException関係のクラスオブジェクト
    - **戻り値**
        - `ResponseEntity` クラスオブジェクト
    - **制約**
        - `GlobalExceptionHandler` は業務例外の場合は `BusinessException` を1回だけ `@ExceptionHandler` し、例外が保持する値を取り出して共通の `ErrorResponse` を組み立てて返す。ただし `BusinessException` を継承した `ValidationException` は項目別のエラーを持つため、別途 `@ExceptionHandler` で処理し `ValidationErrorResponse` を返す。その他の例外は一つずつ記述を行う。
        - `@RestControllerAdvice` ・`@ExceptionHandler` を使用する
        - すべての `@ExceptionHandler` 内で、上記ログ出力ルールに従いログを出力してからレスポンスを返す
        - 存在しないURLへのアクセス(`NoResourceFoundException`)は `404 NOT_FOUND`・errorCode `RESOURCE_NOT_FOUND` で返す(個別に処理しないと `Exception` のハンドラーに拾われ500になるため)
        - 想定外の例外(`Exception`)は `500 INTERNAL_SERVER_ERROR`・errorCode `INTERNAL_SERVER_ERROR`・固定メッセージで返す

---

### security

Spring Securityなどの認証・認可設定、認証フィルター、ユーザー詳細サービス（`UserDetailsService`）の実装クラスなどを配置します。

- **サブパッケージ**
    - jwt/
        - jwtに関するフィルタークラス・サービスクラスなどを配置する
    - cookie/
        - cookieの設定ファイルやcookie周りのサービスクラスなどを配置する
    - userdetails/
        - userdetails関係のファイルを配置する
        - UserDetailsServiceもこのパッケージに配置する
    - handler/
        - セキュリティ・認証周りのハンドラーを配置する
    - config/
        - SecurityConfigなどの設定ファイルを配置する
- **制約**
    - セキュリティに関する設定は、このパッケージ内に集約し、他パッケージから設定を上書きできないようにする。
    - exception関係のファイルは`exception/auth` パッケージに配置する
    - handler関係のファイルは`security/handler` に配置する
    - サブパッケージに該当しないファイルは`security` パッケージ直下に配置する

---

### util

プロジェクト全体で使い回す、状態を持たない静的メソッド（`static method`）のユーティリティクラスを配置します。

- **引数**
    - 指定なし
- **戻り値**
    - 指定なし
- **制約**
    - コンストラクタは実装しない
    - 業務ロジックは記述せず、純粋なデータ加工（日付フォーマット、文字列操作など）のみを行う。
    - 基本はstaticメソッドで実装する

### config

MyBatisやSpring Securityの設定を除く、各種ライブラリ・フレームワークの設定クラス（`@Configuration`）を配置する。

- **フィールド**
    - 各設定に必要な変数を定義する
- **引数・戻り値**
    - 各設定に必要な値を設定する
- **制約**
    - 業務ロジックを記述してはならない。ライブラリ・フレームワークの初期化・設定に限定する。
    - 認証・認可に関する設定は `security` パッケージに記述し、本パッケージには含めない。
    - 1ライブラリ（1関心事）につき1つの設定クラスを作成し、複数ライブラリの設定を1クラスにまとめない。
    - フィールドの値は可能な限り定数で定義する

### doc

springdoc-openapi（Swagger）で表示するAPI仕様書の定義を配置する。
Controllerには仕様書の記述を直接書かず、この層のアノテーションを付与するだけにする。

- **ディレクトリ構成**

```
  doc/
  ├── SampleApiDoc.java
  ├── UserApiDoc.java
  └── AuthApiDoc.java
```

- **ファイル構成・命名**
  * Controllerと1対1でファイルを作成する
  * クラス名は `{Controller名からControllerを除いた名前}ApiDoc` とする
    + 例：`SampleController` → `SampleApiDoc`
  * 1つのApiDocクラスの中に、エンドポイント（Controllerのメソッド）ごとにアノテーション（`@interface`）を定義する
  * アノテーション名はHTTPメソッド名（`Get`, `Post`, `Put`, `Patch`, `Delete`）とする
    + 同じHTTPメソッドが複数ある場合は用途を付ける（例：`GetList`, `GetById`）

- **記述内容**
  * `@Operation`：`summary`（一言）と `description`（テキストブロック）を必ず記述する
    + `description` には処理の概要・必須項目・エラー条件などを箇条書きで書く
  * `@RequestBody`：リクエストボディがある場合に記述する
    + `content` の `schema` と `@ExampleObject` を使い、**リクエストのJSON例はこのファイルに直接書く**
  * `@Parameter`：`@PathVariable`・`@RequestParam` がある場合に記述する（`name`・`in`・`description`・`example`）
  * `@ApiResponse`：返しうるステータスコードごとに記述する
    + 成功時：`content` の `schema` と `@ExampleObject` で **レスポンスのJSON例をこのファイルに直接書く**
    + エラー時：`ErrorResponse`（バリデーションエラーは `ValidationErrorResponse`）を `schema` に指定し、エラーのJSON例を書く
    + ステータスコードは「HTTPSTATUS」の表に従う

- **制約**
  * Controllerクラスに付けてよいのは `@Tag`（クラス）とApiDocのアノテーション（メソッド）のみ
    + `@Operation`・`@ApiResponse` などをControllerに直接書かない
  * `dto/request`・`dto/response` のフィールドに `@Schema` を付与しない（仕様書の記述はこの層に集約する）
  * アノテーションには `@Target(ElementType.METHOD)` と `@Retention(RetentionPolicy.RUNTIME)` を必ず付ける
  * 業務ロジックは記述しない
  * 仕様書の文言はすべて日本語で記述する
  * Controllerのエンドポイントを追加・変更したら、対応するApiDocも同時に更新する

- **コード例**

  ApiDoc

```java
  package com.example.project.doc;

  import io.swagger.v3.oas.annotations.Operation;
  import io.swagger.v3.oas.annotations.media.Content;
  import io.swagger.v3.oas.annotations.media.ExampleObject;
  import io.swagger.v3.oas.annotations.media.Schema;
  import io.swagger.v3.oas.annotations.parameters.RequestBody;
  import io.swagger.v3.oas.annotations.responses.ApiResponse;

  import java.lang.annotation.ElementType;
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;
  import java.lang.annotation.Target;

  /**
   * SampleControllerのAPI仕様書定義
   */
  public class SampleApiDoc {

      /** サンプルデータ取得 */
      @Target(ElementType.METHOD)
      @Retention(RetentionPolicy.RUNTIME)
      @Operation(summary = "サンプルデータ取得", description = """
          固定のサンプルJSONを返すエンドポイント。
          動作確認・疎通確認用途のダミーAPI。
          """)
      @ApiResponse(responseCode = "200", description = "取得成功",
          content = @Content(
              mediaType = "application/json",
              schema = @Schema(implementation = SampleResponseDto.class),
              examples = @ExampleObject(name = "成功", value = """
                  {
                    "id": 1,
                    "name": "サンプル太郎",
                    "email": "sample@example.com",
                    "active": true
                  }
                  """)))
      public @interface Get {
      }

      /** サンプルデータ作成 */
      @Target(ElementType.METHOD)
      @Retention(RetentionPolicy.RUNTIME)
      @Operation(summary = "サンプルデータ作成", description = """
          リクエストされたname, emailをもとにサンプルデータを作成する。

          - name, email は必須
          - email形式が不正な場合は400を返す
          - activeは常にtrueで作成される
          """)
      @RequestBody(required = true,
          content = @Content(
              mediaType = "application/json",
              schema = @Schema(implementation = SampleRequestDto.class),
              examples = @ExampleObject(name = "リクエスト例", value = """
                  {
                    "name": "サンプル太郎",
                    "email": "sample@example.com"
                  }
                  """)))
      @ApiResponse(responseCode = "200", description = "作成成功",
          content = @Content(
              mediaType = "application/json",
              schema = @Schema(implementation = SampleResponseDto.class),
              examples = @ExampleObject(name = "成功", value = """
                  {
                    "id": 1,
                    "name": "サンプル太郎",
                    "email": "sample@example.com",
                    "active": true
                  }
                  """)))
      @ApiResponse(responseCode = "400", description = "バリデーションエラー",
          content = @Content(
              mediaType = "application/json",
              schema = @Schema(implementation = ValidationErrorResponse.class),
              examples = @ExampleObject(name = "バリデーションエラー", value = """
                  {
                    "errorCode": "VALIDATION_ERROR",
                    "message": "入力内容に誤りがあります",
                    "errors": {
                      "email": "メールアドレスの形式が正しくありません"
                    }
                  }
                  """)))
      public @interface Post {
      }
  }
```

  Controller

```java
  @RestController
  @RequestMapping("/api/sample")
  @Tag(name = "Sample", description = "動作確認用のサンプルAPI")
  public class SampleController {

      @GetMapping
      @SampleApiDoc.Get
      public SampleResponseDto getSample() {
          ...
      }

      @PostMapping
      @SampleApiDoc.Post
      public SampleResponseDto createSample(@Valid @RequestBody SampleRequestDto requestDto) {
          ...
      }
  }
```

## 参考記事

- https://beetle2001.hatenablog.com/entry/2026/03/18/202040
- https://tomoblog.net/programing/java/springboot-directory/#google_vignette
- https://saycon.co.jp/archives/neta/spring-bootのdto設計：適切な切り分け方（新人エンジニア

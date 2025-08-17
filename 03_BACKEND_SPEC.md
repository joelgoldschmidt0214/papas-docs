# 03_BACKEND_SPEC.md: バックエンド仕様書 兼 API仕様書

## 1. 技術スタック
- **フレームワーク:** FastAPI (Python 3.12)
- **データベース(ORM):** SQLAlchemy
- **コード品質:** Ruff

## 2. API設計思想
- RESTful APIの原則に従う。
- URLは `/api/v1/` から開始する。
- リクエストとレスポンスのボディはJSON形式とする。
- 認証が必要なエンドポイントは、JWT認証で保護する。

## 3. 認証フロー
1. **ログイン:** `POST /api/v1/auth/login` で成功すると、バックエンドは`access_token`を`HttpOnly`、`Secure`、および `SameSite` 属性付きのCookieにセットして返す。
2. **認証:** 以降のリクエストはCookieが自動送信され、バックエンドがJWTを検証する。

## 4. APIエンドポイント一覧 (随時更新)

### ユーザー認証
- `POST /api/v1/users`: 新規ユーザー登録
- `POST /api/v1/auth/login`: ログイン

### 掲示板
- `GET /api/v1/posts`: 投稿一覧取得
- `POST /api/v1/posts`: 新規投稿作成
    - **Request Body:** `{ "content": "string", "image_url": "string" }`
    - **Success Response (201):** `{ "id": 1, ... }`
- `GET /api/v1/posts/{post_id}`: 個別投稿取得
- `POST /api/v1/posts/{post_id}/comments`: コメント投稿

--- (以下、他の機能のAPI仕様を追記) ---
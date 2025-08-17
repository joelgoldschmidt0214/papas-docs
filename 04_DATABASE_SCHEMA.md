# 04_DATABASE_SCHEMA.md: データベース設計

## 1. 使用データベース
- **本番環境:** Azure Database for MySQL

## 2. ER図
（ここにMermaid記法や画像でER図を記述・添付）

## 3. テーブル定義詳細

### users
- `id` (PK, int, auto_increment): ユーザーID
- `email` (varchar, unique): メールアドレス
- `password_hash` (varchar): ハッシュ化されたパスワード
- `created_at` (datetime): 作成日時
- `updated_at` (datetime): 更新日時

### profiles
- `user_id` (PK, FK to users.id): ユーザーID
- `nickname` (varchar): 表示名
- `bio` (text): 自己紹介文
- `address_area` (varchar(100), not null): 居住エリア

### posts
- `id` (PK, int, auto_increment): 投稿ID
- `user_id` (FK to users.id): 投稿者ID
- `content` (text): 投稿内容
- `image_url` (varchar, nullable): 画像URL
- `created_at` (datetime): 作成日時
- `updated_at` (datetime): 更新日時

### comments
- `id` (PK, int, auto_increment): コメントID
- `post_id` (FK to posts.id): 投稿ID
- `user_id` (FK to users.id): コメント投稿者ID
- `content` (text): コメント内容
- `created_at` (datetime): 作成日時
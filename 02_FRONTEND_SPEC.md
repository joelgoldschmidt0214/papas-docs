# 02_FRONTEND_SPEC.md: フロントエンド仕様書

## 1. 技術スタック
- **フレームワーク:** Next.js (TypeScript, App Router)
- **UIライブラリ:** Tailwind CSS
- **コード品質:** ESLint, Prettier

## 2. ディレクトリ構造
- `src/app/`: 各ページのコンポーネント
- `src/components/ui/`: ボタンやインプットなど汎用的なUIパーツ
- `src/components/features/`: 掲示板やイベントなど機能単位のコンポーネント
- `src/lib/`: APIクライアントや共通の便利関数

## 3. デザインからの実装フロー (Figma -> Code)
1. **デザイン担当:** Figmaでプロトタイプを作成。レイヤー名やコンポーネント名を分かりやすく命名する。
2. **フロントエンド担当:** VS CodeのFigma for VS Code (Dev Mode) を使用し、デザイン定義（色、フォント、レイアウト、CSSコード）を参照しながら実装を進める。
3. GitHub Copilotに「このFigmaのデザインを参考に、Tailwind CSSを使ってヘッダーコンポーネントを作成して」のように指示し、コーディングを効率化する。

## 4. 画面遷移
- **未ログイン時:** トップページ -> ログイン画面 or 新規登録画面
- **ログイン後:** ホーム画面 -> 掲示板一覧 -> 掲示板詳細 ... (etc)

## 5. API連携
- バックエンドチームが提供する `03_BACKEND_SPEC.md` のAPI仕様に従って、データ連携を実装する。
- 認証にはCookieを利用したJWT認証を採用する。詳細は `03_BACKEND_SPEC.md` を参照。
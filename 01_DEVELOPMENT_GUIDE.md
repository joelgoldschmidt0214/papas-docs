# 01_DEVELOPMENT_GUIDE.md: 共通開発ガイド

## 1. 担当者と役割
- **デザイン担当:** FigmaでのUI/UX設計
- **フロントエンド担当:** Figmaデザインを基にNext.jsで画面を実装
- **バックエンド担当:** FastAPIでAPIを設計・実装
- **データベース担当:** Azureでデータベースを設計・構築

## 2. 開発環境のセットアップ手順
1. VS CodeとDocker Desktopをインストール
2. (Windowsの場合) WSL2を有効化
3. Gitをインストールし、SSHキーを作成してGitHubに登録
4. 各リポジトリをクローン (`git clone git@github.com:...`)
5. VS CodeのDev Containers拡張機能を使って、各リポジトリのコンテナを起動

## 3. ソースコード管理 (Git & GitHub)
- **ブランチ戦略:** GitHub Flowを採用
    1. `main`ブランチから`feature/issue-number-feature-name`形式でブランチを作成
    2. 作業完了後、`main`へPull Requestを作成し、レビュー後にマージ
- **コミットメッセージ:** `feat: ユーザー登録機能を追加` のように、変更内容が分かるように記述する。

## 4. ツールと連携
- **IDE:** VS Code + Dev Containers拡張機能
- **AIアシスタント:** GitHub Copilot (エディタ上で活用)
- **コミュニケーション:** (ここにSlackやTeamsなど、使用するツールを記載)
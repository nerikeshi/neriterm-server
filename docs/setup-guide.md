# 詳細セットアップガイド

NeriTerm Serverの詳細なセットアップ手順です。

## 目次

- [必要環境](#必要環境)
- [インストール手順](#インストール手順)
- [設定ファイル](#設定ファイル)
- [プッシュ通知の設定](#プッシュ通知の設定)
- [サービス管理](#サービス管理)

## 必要環境

### macOS

- macOS 12.0 (Monterey) 以降

### Node.js

Node.js 18.0以降が必要です。

```bash
# バージョン確認
node -v

# Homebrewでインストール
brew install node

# または nvm を使用
nvm install 18
nvm use 18
```

## インストール手順

### 1. ダウンロード

[リリースページ](https://github.com/nerikeshi/neriterm-server/releases)から最新版をダウンロードします。

### 2. 展開

```bash
cd ~/Downloads
unzip claude-server-v*.zip
cd claude-server-v*-macos
```

### 3. インストール実行

```bash
./install.sh
```

インストールスクリプトが以下を実行します：

1. Node.jsのバージョンチェック
2. `~/.neriterm-server` へのファイルコピー
3. AUTH_TOKENの自動生成
4. LaunchAgentの設定（自動起動）
5. サーバーの起動

### 4. 認証トークンを保存

**重要**: 表示されるAUTH_TOKENを必ず保存してください。

```
========================================
   AUTH_TOKEN Generated
========================================

Your authentication token:

  a1b2c3d4e5f6...(64文字のトークン)

IMPORTANT: Save this token!
You will need to enter it in the NeriTerm iOS app.
========================================
```

### 5. 動作確認

ブラウザで `http://localhost:8080/status` にアクセスするか：

```bash
curl http://localhost:8080/health
# {"status":"ok","timestamp":"..."}
```

## 設定ファイル

設定ファイルは `~/.neriterm-server/.env` にあります。

```bash
# サーバー設定
PORT=8080
AUTH_TOKEN=your-auth-token-here

# Firebase設定（プッシュ通知用、オプション）
FIREBASE_SERVICE_ACCOUNT_PATH=./keys/firebase-service-account.json

# ターミナル設定
DEFAULT_SHELL=/bin/zsh
DEFAULT_WORKING_DIR=/Users/your-username

# アップロード設定
UPLOAD_MAX_AGE_DAYS=15
```

### 設定項目の説明

| 項目 | 説明 | デフォルト |
|------|------|----------|
| PORT | サーバーポート | 8080 |
| AUTH_TOKEN | 認証トークン（必須、32文字以上） | - |
| FIREBASE_SERVICE_ACCOUNT_PATH | Firebase認証キーのパス | - |
| DEFAULT_SHELL | 使用するシェル | /bin/zsh |
| DEFAULT_WORKING_DIR | デフォルトの作業ディレクトリ | $HOME |
| UPLOAD_MAX_AGE_DAYS | アップロード画像の保持日数 | 15 |

## プッシュ通知の設定

プッシュ通知を有効にするには、Firebase Cloud Messagingの設定が必要です。

### 1. Firebaseプロジェクトの作成

1. [Firebase Console](https://console.firebase.google.com/)にアクセス
2. 「プロジェクトを追加」をクリック
3. プロジェクト名を入力して作成

### 2. サービスアカウントキーの取得

1. プロジェクト設定（歯車アイコン）をクリック
2. 「サービスアカウント」タブを選択
3. 「新しい秘密鍵の生成」をクリック
4. JSONファイルをダウンロード

### 3. キーファイルの配置

```bash
# ダウンロードしたファイルを配置
cp ~/Downloads/your-project-firebase-adminsdk-*.json \
   ~/.neriterm-server/keys/firebase-service-account.json
```

### 4. サーバーを再起動

```bash
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
```

## サービス管理

### サーバーの状態確認

```bash
# ヘルスチェック
curl http://localhost:8080/health

# ステータスページ
open http://localhost:8080/status
```

### ログの確認

```bash
# リアルタイムログ
tail -f ~/.neriterm-server/logs/stderr.log

# 最新100行
tail -100 ~/.neriterm-server/logs/stderr.log
```

### サーバーの停止・起動

```bash
# 停止
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist

# 起動
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist

# 再起動（停止→起動）
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist && \
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
```

### アンインストール

```bash
# インストールディレクトリに移動して実行
cd ~/.neriterm-server
./uninstall.sh

# または、展開したパッケージから実行
cd ~/Downloads/claude-server-v*-macos
./uninstall.sh
```

## ディレクトリ構造

```
~/.neriterm-server/
├── dist/                  # サーバーコード
├── node_modules/          # 依存関係
├── data/                  # データ
│   ├── projects.json      # プロジェクト一覧
│   └── device-tokens.json # デバイストークン
├── keys/                  # 認証キー
│   └── firebase-service-account.json
├── logs/                  # ログ
│   ├── stdout.log
│   └── stderr.log
├── uploads/               # アップロード画像
└── .env                   # 設定ファイル
```

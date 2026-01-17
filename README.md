# NeriTerm Server

<p align="center">
  <img src="https://img.shields.io/badge/platform-macOS-blue" alt="Platform">
  <img src="https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen" alt="Node.js">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
</p>

NeriTerm iOS アプリ用のコンパニオンサーバーです。
MacでClaude Codeを実行し、iPhoneからリモートアクセスを可能にします。

## 特徴

- **リアルタイム同期** - Claude Codeの出力をリアルタイムでiPhoneに表示
- **プッシュ通知** - 入力待ちや完了を通知（オプション）
- **マルチプロジェクト** - 複数のプロジェクトを簡単に切り替え
- **セキュア接続** - Tailscale経由の暗号化通信

## 必要環境

- macOS 12.0 (Monterey) 以降
- Node.js 18.0 以降
- Tailscale（リモートアクセス用、推奨）

## クイックスタート

### 1. ダウンロード

[最新リリース](https://github.com/nerikeshi/neriterm-server/releases/latest)からZIPファイルをダウンロード

### 2. インストール

```bash
# ZIPを展開
unzip claude-server-v*.zip
cd claude-server-*

# インストール実行
./install.sh
```

### 3. 認証トークンを保存

インストール時に表示される **AUTH_TOKEN** を必ず保存してください。
NeriTermアプリで接続する際に必要です。

### 4. Tailscaleを設定

[Tailscale](https://tailscale.com/)をインストールしてログインすると、
iPhoneからMacにセキュアにアクセスできます。

## ダウンロード

| バージョン | ダウンロード |
|-----------|-------------|
| 最新版 | [ダウンロード](https://github.com/nerikeshi/neriterm-server/releases/latest) |

## ドキュメント

- [詳細セットアップガイド](docs/setup-guide.md)
- [Tailscale設定ガイド](docs/tailscale-setup.md)
- [トラブルシューティング](docs/troubleshooting.md)

## NeriTerm iOS アプリ

App Storeからダウンロード（準備中）

## コマンド

```bash
# サーバーの状態確認
curl http://localhost:8080/health

# ログを確認
tail -f ~/.neriterm-server/logs/stderr.log

# サーバーを再起動
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist

# アンインストール
./uninstall.sh
```

## サポート

- [GitHub Issues](https://github.com/nerikeshi/neriterm-server/issues)
- Email: nerikeshi.cola@gmail.com

## ライセンス

MIT License - 詳細は [LICENSE](LICENSE) ファイルを参照してください。

# Tailscale設定ガイド

Tailscaleを使用すると、外出先からでもiPhoneでMacのNeriTerm Serverに安全にアクセスできます。

## Tailscaleとは

Tailscaleは、WireGuardベースのVPNサービスです。
複雑な設定なしに、デバイス間のセキュアな接続を実現します。

## セットアップ手順

### 1. アカウント作成

1. [Tailscale](https://tailscale.com/)にアクセス
2. 「Get Started」をクリック
3. Google、Microsoft、GitHubなどでサインアップ

### 2. Macにインストール

#### Homebrewの場合

```bash
brew install --cask tailscale
```

#### 手動インストール

1. [ダウンロードページ](https://tailscale.com/download/mac)からインストーラーをダウンロード
2. アプリをApplicationsフォルダにドラッグ
3. Tailscaleを起動

### 3. Macでログイン

1. メニューバーのTailscaleアイコンをクリック
2. 「Log in」をクリック
3. ブラウザで認証

### 4. iPhoneにインストール

1. App Storeから「Tailscale」をインストール
2. アプリを起動してログイン（Macと同じアカウント）

### 5. 接続確認

MacのTailscale IPアドレスを確認します：

```bash
# ターミナルで確認
tailscale ip -4
# 例: 100.123.45.67
```

または、メニューバーのTailscaleアイコンから確認できます。

## NeriTermアプリでの設定

1. NeriTermアプリを開く
2. サーバー設定画面で以下を入力：
   - **ホスト**: `100.x.x.x`（TailscaleのIPアドレス）
   - **ポート**: `8080`
   - **AUTH_TOKEN**: インストール時に生成されたトークン
3. 「接続」をタップ

## ネットワーク構成

```
┌─────────────┐     Tailscale      ┌─────────────┐
│   iPhone    │◄──────VPN────────►│     Mac     │
│  NeriTerm   │    (暗号化通信)     │   Server    │
│             │                    │  Port 8080  │
└─────────────┘                    └─────────────┘
   100.x.x.y                          100.x.x.z
```

## セキュリティ

### Tailscaleの利点

- **エンドツーエンド暗号化**: WireGuardによる強力な暗号化
- **ゼロトラスト**: 認証されたデバイスのみアクセス可能
- **NAT越え**: ポート開放不要
- **監査ログ**: 接続履歴の確認が可能

### 推奨設定

1. **MagicDNSを有効化**
   - Tailscale管理画面 → DNS → Enable MagicDNS
   - IPアドレスの代わりにホスト名で接続可能に

2. **キー有効期限の設定**
   - 定期的なキーローテーションを有効化

## トラブルシューティング

### 接続できない場合

1. **両方のデバイスがTailscaleに接続されているか確認**
   ```bash
   tailscale status
   ```

2. **Tailscaleが起動しているか確認**
   - Mac: メニューバーにアイコンがあるか
   - iPhone: VPNアイコンが表示されているか

3. **ファイアウォールの確認**
   - macOSのファイアウォールでTailscaleを許可

### IPアドレスが変わった場合

TailscaleのIPアドレスは通常固定ですが、再インストールすると変わることがあります。
新しいIPアドレスをNeriTermアプリで更新してください。

### MagicDNSを使用する場合

MagicDNSを有効にすると、以下のようにホスト名でアクセスできます：

```
mac-name.tail12345.ts.net:8080
```

## 料金

- **個人利用**: 無料（最大100デバイス）
- 詳細は [Tailscale Pricing](https://tailscale.com/pricing/) を参照

## 参考リンク

- [Tailscale公式ドキュメント](https://tailscale.com/kb/)
- [macOS設定ガイド](https://tailscale.com/kb/1016/install-mac/)
- [iOS設定ガイド](https://tailscale.com/kb/1020/install-ios/)

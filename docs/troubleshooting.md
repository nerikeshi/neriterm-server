# トラブルシューティング

NeriTerm Serverで問題が発生した場合の対処方法です。

## 目次

- [インストール時の問題](#インストール時の問題)
- [接続の問題](#接続の問題)
- [認証の問題](#認証の問題)
- [サーバーの問題](#サーバーの問題)
- [プッシュ通知の問題](#プッシュ通知の問題)

## インストール時の問題

### Node.jsがインストールされていない

```
Error: Node.js is not installed.
```

**解決方法:**

```bash
# Homebrewでインストール
brew install node

# または公式サイトからダウンロード
# https://nodejs.org/
```

### Node.jsのバージョンが古い

```
Error: Node.js 18 or higher is required.
Current version: v16.x.x
```

**解決方法:**

```bash
# Homebrewでアップグレード
brew upgrade node

# nvmを使用している場合
nvm install 18
nvm use 18
```

### 権限エラー

```
Permission denied
```

**解決方法:**

```bash
# install.shに実行権限を付与
chmod +x install.sh
./install.sh
```

## 接続の問題

### サーバーに接続できない（ローカル）

**確認手順:**

1. サーバーが起動しているか確認
   ```bash
   curl http://localhost:8080/health
   ```

2. サービスの状態を確認
   ```bash
   launchctl list | grep neriterm
   ```

3. ログを確認
   ```bash
   tail -50 ~/.neriterm-server/logs/stderr.log
   ```

### サーバーに接続できない（リモート/Tailscale）

**確認手順:**

1. Tailscaleが両方のデバイスで接続されているか
   ```bash
   tailscale status
   ```

2. MacのTailscale IPを確認
   ```bash
   tailscale ip -4
   ```

3. Tailscale経由で接続テスト
   ```bash
   # iPhoneのターミナルアプリから、またはMacから
   curl http://100.x.x.x:8080/health
   ```

### WebSocket接続が切れる

**考えられる原因:**

- ネットワークの不安定さ
- サーバーのリソース不足
- Tailscaleの接続断

**解決方法:**

1. サーバーを再起動
   ```bash
   launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
   launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
   ```

2. Tailscaleを再接続
   - メニューバーからDisconnect → Connect

## 認証の問題

### AUTH_TOKENを忘れた

**解決方法:**

```bash
# .envファイルからトークンを確認
cat ~/.neriterm-server/.env | grep AUTH_TOKEN
```

### 新しいAUTH_TOKENを生成したい

**解決方法:**

```bash
# 新しいトークンを生成
NEW_TOKEN=$(openssl rand -hex 32)
echo "New token: $NEW_TOKEN"

# .envファイルを編集
nano ~/.neriterm-server/.env
# AUTH_TOKEN=の行を新しいトークンに更新

# サーバーを再起動
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
```

### 認証エラーが発生する

```
Authentication failed
```

**確認手順:**

1. トークンが正しいか確認
2. トークンが32文字以上か確認
3. トークンの前後に空白がないか確認

## サーバーの問題

### サーバーが起動しない

**確認手順:**

1. ログを確認
   ```bash
   tail -100 ~/.neriterm-server/logs/stderr.log
   ```

2. 手動で起動してエラーを確認
   ```bash
   cd ~/.neriterm-server
   node dist/index.js
   ```

3. ポートが使用中でないか確認
   ```bash
   lsof -i :8080
   ```

### ポート8080が使用中

**解決方法:**

1. 使用中のプロセスを確認・終了
   ```bash
   lsof -i :8080
   kill -9 <PID>
   ```

2. または別のポートを使用
   ```bash
   # .envファイルを編集
   nano ~/.neriterm-server/.env
   # PORT=8081 などに変更
   
   # サーバーを再起動
   launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
   launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
   ```

### メモリ使用量が多い

**確認手順:**

```bash
# プロセスのメモリ使用量を確認
ps aux | grep node
```

**解決方法:**

サーバーを再起動してメモリを解放

```bash
launchctl unload ~/Library/LaunchAgents/com.neriterm.claude-server.plist
launchctl load ~/Library/LaunchAgents/com.neriterm.claude-server.plist
```

## プッシュ通知の問題

### 通知が届かない

**確認手順:**

1. Firebaseの設定を確認
   ```bash
   ls -la ~/.neriterm-server/keys/firebase-service-account.json
   ```

2. ログでFCMエラーを確認
   ```bash
   grep -i "fcm\|firebase\|notification" ~/.neriterm-server/logs/stderr.log
   ```

3. iPhoneの通知設定を確認
   - 設定 → NeriTerm → 通知 → 許可

### Firebase設定エラー

```
Firebase service account file not found
```

**解決方法:**

1. JSONファイルが正しい場所にあるか確認
2. ファイル名とパスが.envの設定と一致しているか確認
3. JSONファイルの内容が正しいか確認

## ログの確認方法

### リアルタイムログ

```bash
tail -f ~/.neriterm-server/logs/stderr.log
```

### 特定のエラーを検索

```bash
grep -i "error" ~/.neriterm-server/logs/stderr.log
```

### 最近のログ

```bash
tail -200 ~/.neriterm-server/logs/stderr.log
```

## 完全な再インストール

問題が解決しない場合は、完全な再インストールを試してください：

```bash
# 1. アンインストール
./uninstall.sh
# すべてのオプションで「y」を選択

# 2. 残存ファイルの確認・削除
rm -rf ~/.neriterm-server
rm -f ~/Library/LaunchAgents/com.neriterm.claude-server.plist

# 3. 再インストール
cd /path/to/claude-server-v*-macos
./install.sh
```

## サポート

上記で解決しない場合は、以下の情報を添えてお問い合わせください：

1. macOSのバージョン
2. Node.jsのバージョン (`node -v`)
3. エラーログ (`~/.neriterm-server/logs/stderr.log`の最後100行)
4. 実行したコマンドと出力

- [GitHub Issues](https://github.com/nerikeshi/neriterm-server/issues)
- Email: nerikeshi.cola@gmail.com

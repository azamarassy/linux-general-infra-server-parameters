# SSH サーバー設定定義

## 1. 基本構成情報

SSHサービスの動作に必要なパッケージおよびサービスの情報です。

| 項目 | 値 | 取得コマンド例 |
| --- | --- | --- |
| **パッケージ名** | openssh-8.0p1-25.el8_10.x86_64 | `rpm -qa | grep ssh` |
| **サービス名** | sshd.service | `systemctl status sshd` |
| **設定ファイル** | `/etc/ssh/sshd_config` | `ls -l /etc/ssh/` |

---

## 2. 詳細設定項目 (`/etc/ssh/sshd_config`)

セキュリティおよび接続挙動に関する具体的な設定内容です。

| 項目 | 設定値 | 説明 |
| --- | --- | --- |
| **Port** | 22 (デフォルト) | SSHサーバーが待ち受けるポート番号。 |
| **PermitRootLogin** | **yes** | rootでの直接ログインを許可。セキュリティの観点で見直しが推奨される場合あり。 |
| **PasswordAuthentication** | **yes** | パスワード認証を許可。 |
| **ChallengeResponseAuthentication** | **no** | チャレンジレスポンス認証を無効化。 |
| **AuthorizedKeysFile** | `.ssh/authorized_keys` | 公開鍵認証で使用するファイルのパスを指定。 |
| **UsePAM** | **yes** | PAM（Pluggable Authentication Module）を有効化。 |
| **GSSAPIAuthentication** | **yes** | GSSAPIによる認証を許可。 |
| **X11Forwarding** | **yes** | X11転送を許可。GUIアプリケーションの使用に必要な場合に有効。 |
| **PrintMotd** | **no** | ログイン時のMOTD（Message Of The Day）メッセージを非表示に設定。 |
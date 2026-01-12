# sudo 権限設定定義 (`/etc/sudoers`)

## 1. 設定の基本情報

| 項目 | 値 |
| --- | --- |
| **設定ファイルパス** | `/etc/sudoers` |
| **編集用コマンド** | `visudo` (推奨) |

---

## 2. コマンドエイリアス (Cmnd_Alias) 定義

特定の役割ごとに許可、または制限するコマンドをグループ化しています。

| エイリアス名 | カテゴリ | 含まれるコマンド例 |
| --- | --- | --- |
| **DEV_TOOLS** | 開発ツール | `git`, `make`, `gcc`, `python3`, `pip`, `npm` |
| **DEV_SERVICE** | サービス再起動 | `systemctl restart apache2 / php-fpm` |
| **DEV_MONITOR** | ログ確認 | `journalctl (apache2 / php-fpm)` |
| **DEV_CONFIG** | 権限操作 | `chmod`, `chown` |
| **REBOOT** | システム停止 | `reboot`, `shutdown` |
| **USER_ADMIN** | ユーザー管理 | `useradd`, `usermod`, `userdel` 等 |
| **SU_ROOT** | 特権昇格 | `su` |
| **SYS_CONTROL** | 基盤制御 | `systemctl restart network` |

---

## 3. ユーザーグループ別 権限割り当て

各グループに適用される具体的なsudoルールです。

| 対象グループ | 実行時パスワード | 許可されるアクション / 制限事項 |
| --- | --- | --- |
| **%developers** | **必要** | **限定許可**(開発ツール、指定サービスの再起動、ログ確認、権限変更のみ実行可能) |
| **%admins** | **不要 (NOPASSWD)** | **原則全許可**(全てのコマンドを実行可能だが、誤操作防止のため REBOOT, USER_ADMIN, SU_ROOT, SYS_CONTROL は禁止)|

---

## 4. 実際の設定内容 (Config Raw)

```text
# 開発者向けの許可されたコマンドセット
Cmnd_Alias DEV_TOOLS = /usr/bin/git, /usr/bin/make, /usr/bin/gcc, /usr/bin/python3, /usr/bin/pip, /usr/bin/npm
Cmnd_Alias DEV_SERVICE = /bin/systemctl restart apache2, /bin/systemctl restart php-fpm
Cmnd_Alias DEV_MONITOR = /bin/journalctl --unit=apache2 --no-pager, /bin/journalctl --unit=php-fpm --no-pager
Cmnd_Alias DEV_CONFIG = /bin/chmod, /bin/chown

# 開発者は、開発関連ツール・アプリケーションの再起動・ログ確認・権限変更のみ許可
%developers ALL=(ALL) DEV_TOOLS, DEV_SERVICE, DEV_MONITOR, DEV_CONFIG

# 管理者向けの制限付き sudo コマンドセット
Cmnd_Alias REBOOT = /sbin/reboot, /sbin/shutdown
Cmnd_Alias USER_ADMIN = /usr/sbin/useradd, /usr/sbin/usermod, /usr/sbin/userdel, /usr/sbin/groupadd, /usr/sbin/groupmod, /usr/sbin/groupdel
Cmnd_Alias SU_ROOT = /bin/su
Cmnd_Alias SYS_CONTROL = /bin/systemctl restart network

# 管理者はパスワードなしですべてのコマンドを実行可能だが、上記の特定コマンドは禁止(誤操作、セキュリティリスク防止)
%admins ALL=(ALL) NOPASSWD: ALL, !REBOOT, !USER_ADMIN, !SU_ROOT, !SYS_CONTROL

```
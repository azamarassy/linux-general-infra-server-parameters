# ログローテート設定定義

## 1. グローバル設定 (`/etc/logrotate.conf`)

システム全体のデフォルト挙動に関する設定です。

| 項目 | デフォルト | CIS推奨 | 設定値 |
| --- | --- | --- | --- |
| **ローテート周期** | 週次 | 週次 | **週次** |
| **保存世代数** | 4世代 | 4世代 | **4世代** |
| **空ファイルの作成** | 作成する | 作成する | **作成する** |
| **日付付与 (YYYYMMDD)** | つける | つける | **つける** |
| **圧縮設定 (compress)** | 無効化 | 有効化 | **有効化** |
| **インクルードディレクトリ** | `/etc/logrotate.d/` | `/etc/logrotate.d/` | **`/etc/logrotate.d/`** |

---

## 2. 個別設定一覧 (`/etc/logrotate.d/*`)

各サービスごとの具体的なローテートポリシーです。

| 対象サービス | 設定ファイル名 | 周期 | 世代数 | 空ファイル作成 | 日付付与 | 圧縮 |
| --- | --- | --- | --- | --- | --- | --- |
| **Apache (httpd)** | `/etc/logrotate.d/httpd` | 週次 | 4 | 作成する | つける | 有効 |
| **PHP (php-fpm)** | `/etc/logrotate.d/php-fpm` | 週次 | 4 | 作成する | つける | 有効 |
| **SSH (sshd)** | `/etc/logrotate.d/sshd` | 月次 | 6 | 作成する | つける | 有効 |
| **Firewall (firewalld)** | `/etc/logrotate.d/firewalld` | 月次 | 6 | 作成する | つける | 有効 |
| **Syslog (rsyslog)** | `/etc/logrotate.d/syslog` | 日次 | 6 | 作成する | つける | 有効 |
| **Zabbix Agent** | `/etc/logrotate.d/zabbix-agent` | 週次 | 4 | 作成する | つける | 有効 |
| **Chrony (NTP)** | `/etc/logrotate.d/chrony` | 月次 | 6 | 作成する | つける | 有効 |
| **psacct** | (個別設定) | 日次 | 31 | `create 0600 root root` | つける | 有効 |
| **wpa_supplicant** | (個別設定) | - | 1 | `create 0600 root root` | - | - |
| **wtmp** | (個別設定) | 月次 | 1 | `create 0664 root utmp` | - | - |

---

### 補足事項

* **postrotate処理**: ApacheやSyslogなど、ログファイルを切り替えた後にサービスの再起動やシグナルの送信（HUP）が必要な項目については、各設定ファイル内の `postrotate` セクションにて定義されます。
* **wtmp/btmp**: ログイン履歴などのバイナリログは、セキュリティ監査の観点から通常とは異なる所有者権限（utmpグループ等）で作成されるよう指定されています。


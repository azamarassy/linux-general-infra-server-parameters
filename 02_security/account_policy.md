# セキュリティ確認基準・コマンド一覧

## 1. アカウントのロック

| 項目 | デフォルト | CIS | 設定値 | 取得コマンド例 |
| --- | --- | --- | --- | --- |
| 許可されるログイン試行回数 | 設定なし | 3回まで | 5回まで | `grep "pam_faillock.so" /etc/pam.d/password-auth /etc/pam.d/system-auth` |
| アカウントのロックアウト期間 | 設定なし | 15分 | 5分 | `grep "unlock_time" /etc/security/faillock.conf` |
| 試行回数のカウントリセット時間 | 設定なし | 30分 | 60分 | `grep "fail_interval" /etc/security/faillock.conf` |

---

## 2. パスワードポリシー

| 項目 | デフォルト | CIS | 設定値 | 取得コマンド例 |
| --- | --- | --- | --- | --- |
| パスワードの有効期限 | 無期限 | 60日 | 60日 | `chage -l root` |
| 期限切れ警告表示 | 7日 | 14日前 | 14日前 | `chage -l root` |
| パスワード変更不可能期間 | 設定なし | 7日 | 7日 | `chage -l root` |
| 過去のパスワード利用制限 | 設定なし | 10回分 | 10回分 | `grep remember /etc/security/pwquality.conf` |
| パスワードの最小文字数 | 設定なし | 14文字 | 14文字 | `grep minlen /etc/security/pwquality.conf` |
| 英字大文字利用 | 設定なし | 必須 | 必須 | `grep ucredit /etc/security/pwquality.conf` |
| 英字小文字利用 | 設定なし | 必須 | 必須 | `grep lcredit /etc/security/pwquality.conf` |
| 数字利用 | 設定なし | 必須 | 必須 | `grep dcredit /etc/security/pwquality.conf` |
| 記号利用 | 設定なし | 必須 | 必須 | `grep ocredit /etc/security/pwquality.conf` |
| 連続する同文字の最大数 | 設定なし | 2文字まで | 2文字まで | `grep maxrepeat /etc/security/pwquality.conf` |
| 連続する同種文字の最大数 | 設定なし | 3文字まで | 3文字まで | `grep maxclassrepeat /etc/security/pwquality.conf` |
| パスワードの辞書チェック | 設定なし | 有効 | 有効 | `grep dictcheck /etc/security/pwquality.conf` |
| ユーザー名の含有チェック | 設定なし | 有効 | 有効 | `grep usercheck /etc/security/pwquality.conf` |

---

## 3. root管理
| 項目 | デフォルト | 設定値 | 取得コマンド例 |
| --- | --- | --- | --- |
| 一般ユーザからのroot昇格 | 可能 | 無効 | `cat /etc/sudoers` |


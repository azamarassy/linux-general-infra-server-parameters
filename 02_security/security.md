# Linux セキュリティ・認証基盤設定定義

## 1. PAM認証設定 (`/etc/pam.d/`)

### 1.1 system-auth

ローカルログイン、SSH、コンソール等、システム全体のデフォルト認証フローを定義します。

| セクション | モジュール | 制御フラグ | 説明 |
| --- | --- | --- | --- |
| **auth** (認証) | pam_env.so | required | 環境変数の設定 |
|  | pam_faildelay.so | required | 認証失敗時の遅延（2秒） |
|  | pam_fprintd.so | sufficient | 指紋認証対応 |
|  | pam_usertype.so | sufficient | 通常ユーザーの確認 |
|  | pam_localuser.so | sufficient | ローカルユーザーの確認 |
|  | pam_unix.so | sufficient | UNIX認証（パスワード空でも許可） |
|  | pam_sss.so | sufficient | SSSによるネットワーク認証 |
|  | pam_deny.so | required | デフォルトで拒否 |
| **account** (管理) | pam_unix.so | required | UNIX方式のアカウント確認 |
|  | pam_localuser.so | sufficient | ローカルユーザーのみ許可 |
|  | pam_usertype.so | sufficient | システムユーザーの確認 |
|  | pam_sss.so | sufficient | SSSによるアカウント情報の確認 |
|  | pam_permit.so | required | 常に許可 |
| **password** (変更) | pam_pwquality.so | requisite | パスワード品質チェック |
|  | pam_unix.so | sufficient | パスワード暗号化（SHA512） |
|  | pam_sss.so | sufficient | SSSを使用したパスワード管理 |
|  | pam_deny.so | required | デフォルトで拒否 |
| **session** (環境) | pam_keyinit.so | optional | セッションのキーリング初期化 |
|  | pam_limits.so | required | ユーザー/プロセスのリソース制限 |
|  | pam_systemd.so | optional | Systemdセッションの初期化 |
|  | pam_succeed_if.so | sufficient | crondサービスに関する条件管理 |
|  | pam_unix.so | required | UNIX方式のセッション管理 |
|  | pam_sss.so | optional | SSSを使用したセッション管理 |

### 1.2 password-auth

リモートアクセス（SSH等）の認証に使用される設定です。

| セクション | 制御フラグ | 説明 |
| --- | --- | --- |
| **auth** | sufficient | pam_unix.so (UNIX認証), pam_sss.so (SSS認証) |
| **account** | sufficient | pam_localuser.so (ローカルユーザー限定) |
| **password** | sufficient | pam_unix.so (SHA512), pam_sss.so (トークン適用) |
| **session** | required | pam_limits.so (リソース制限), pam_unix.so (セッション管理) |

---

## 2. アカウントロック設定 (`/etc/security/faillock.conf`)

連続ログイン失敗時の挙動を定義します。

| 項目 | 説明 | 設定値 |
| --- | --- | --- |
| **dir** | 失敗記録ディレクトリ | `/var/run/faillock` |
| **audit** | ログ記録 | ユーザー未検出時にユーザー名を記録 |
| **silent** | メッセージ非表示 | 認証失敗時のメッセージを表示しない |
| **no_log_info** | syslog記録無効化 | 情報をsyslogに記録しない |
| **local_users_only** | ローカルユーザー追跡 | ローカルユーザーのみを対象とする |
| **deny** | 失敗回数制限 | **3** (3回以上でロック) |
| **fail_interval** | 失敗間隔 | **900** (秒) |
| **unlock_time** | 再アクセス許可時間 | **600** (秒) |
| **even_deny_root** | Rootロック | **有効** (Rootもロック対象) |
| **root_unlock_time** | Root再アクセス時間 | **900** (秒) |

---

## 3. システム・パスワード基本設定 (`/etc/login.defs`)

新規ユーザー作成時やパスワードのデフォルト有効期限を定義します。

| 項目 | 説明 | 設定値 / デフォルト |
| --- | --- | --- |
| **MAIL_DIR** | メールボックス保存先 | `/var/spool/mail` |
| **UMASK** | 初期UMASK値 | `022` |
| **HOME_MODE** | ホームディレクトリモード | `0700` |
| **PASS_MAX_DAYS** | パスワード最大有効日数 | `99999` |
| **PASS_MIN_DAYS** | パスワード最小間隔日数 | `0` |
| **PASS_MIN_LEN** | パスワード最小文字数 | `5` |
| **PASS_WARN_AGE** | パスワード警告期間 | `7` |
| **UID_MIN / MAX** | ユーザーUID範囲 | `1000` / `60000` |
| **SYS_UID_MIN / MAX** | システムUID範囲 | `201` / `999` |
| **CREATE_HOME** | ホームディレクトリ作成 | `yes` |
| **ENCRYPT_METHOD** | 暗号化方式 | `SHA512` |
| **SHA_CRYPT_MAX_ROUNDS** | ストレッチング回数 | `5000` |

---

## 4. パスワード品質の詳細設定 (`/etc/security/pwquality.conf`)

| 項目 | 説明 | 設定値 / デフォルト |
| --- | --- | --- |
| **difok** | 変更時異なる文字数 | `1` |
| **minlen** | 最小パスワード長 | **8** |
| **dcredit / ucredit** | 数字 / 大文字制限 | `0` (クレジット方式) |
| **lcredit / ocredit** | 小文字 / 記号制限 | `0` (クレジット方式) |
| **minclass** | 必要な文字種数 | `0` |
| **maxrepeat** | 同一文字連続最大数 | `0` (無効) |
| **dictcheck** | 辞書検査 | **1** (有効) |
| **usercheck** | ユーザー名検査 | **1** (有効) |
| **enforcing** | 強制チェック | **1** (有効) |
| **retry** | 再試行回数 | `3` |

---

## 5. root昇格制限設定 (`/etc/pam.d/su`)

`su` コマンドによる特権昇格の挙動を制御します。

| セクション | モジュール | 内容・備考 |
| --- | --- | --- |
| **auth** | pam_rootok.so | sufficient (Rootからのsuはパスワード不要) |
| **auth** | pam_wheel.so | (コメントアウト) 有効化でwheelグループのみ昇格許可 |
| **auth** | substack | system-auth を参照 |
| **account** | pam_succeed_if.so | uid=0 (Root) の場合に成功とする条件 |
| **session** | optional pam_xauth.so | X11転送の認証情報を引き継ぐ設定 |


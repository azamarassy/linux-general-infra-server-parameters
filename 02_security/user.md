# ユーザー管理一覧

## 1. ユーザー属性・権限設定

システムに登録されている各ユーザーの権限およびアクセス範囲の定義です。

| ユーザー名 | UID | ユーザー権限 | ユーザー種別 | 特権 (sudo等) |
| --- | --- | --- | --- | --- |
| **Admin_user** | 1001 | 管理者 | システム管理 | sudo使用可 |
| **Developper** | 1002 | 開発者 | 開発者 | sudo制限付き [*1] |
| **General_user** | 1003 | 一般ユーザー | 一般 | なし |

---

## 2. 所属グループ・環境設定

| ユーザー名 | プライマリグループ | セカンダリグループ | ホームディレクトリ | ログインシェル |
| --- | --- | --- | --- | --- |
| **Admin_user** | admins | wheel, apache | /home/admin_user | /bin/bash |
| **Developper** | developers | apache | /home/developper | /bin/bash |
| **General_user** | users | apache | /home/general_user | /bin/bash |

---

## 3. パスワード・セキュリティ設定

| ユーザー名 | パスワード入力 | パスワード有効期限 |
| --- | --- | --- |
| **Admin_user** | 必要 | 90日 |
| **Developper** | 必要 | 90日 |
| **General_user** | 必要 | 90日 |

---

### 補足事項

<a id="sudo_limit"></a>
**[*1] 開発者ユーザーのsudo制限内容:**
以下のコマンドに限定して実行を許可：

* `/usr/bin/git`
* `/usr/bin/make`
* `/usr/bin/gcc`

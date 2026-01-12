# 時刻同期（chrony）設定定義

## 1. 基本構成情報

システムの時刻精度を維持するためのパッケージおよびサービスの情報です。

| 項目 | 値 | 取得コマンド例 |
| --- | --- | --- |
| **パッケージ名** | chrony-4.5-2.el8_10.x86_64 | `rpm -qa | grep chrony` |
| **サービス名** | chronyd.service | `systemctl status chronyd` |
| **設定ファイル** | `/etc/chrony.conf` | `ls -l /etc/chrony.conf` |

---

## 2. 詳細設定項目 (`/etc/chrony.conf`)

時刻同期の参照先や、システムクロックへの反映方法に関する設定です。

| 項目 | 設定値 | 説明 |
| --- | --- | --- |
| **pool** | `2.rocky.pool.ntp.org iburst` | 公共のNTPサーバーを利用して時刻同期を行う設定。 |
| **driftfile** | `/var/lib/chrony/drift` | システムクロックが進む/遅れる割合を記録し、再起動後の補正に利用する。 |
| **makestep** | `1.0 3` | オフセットが1秒以上の場合、最初の3回の更新に限り、時間を飛ばして（ステップ）修正する。 |
| **rtcsync** | 有効 | リアルタイムクロック（RTC）の同期をカーネルレベルで有効化する。 |
| **keyfile** | `/etc/chrony.keys` | NTP認証に使用するキーを格納するファイルのパス。 |
| **leapsectz** | `right/UTC` | システムのタイムゾーンデータベースから閏秒情報を取得する設定。 |
| **logdir** | `/var/log/chrony` | Chronyが動作ログファイルを保存するディレクトリのパス。 |

---

### 補足事項

* **同期状態の確認**:
現在の同期先や精度を確認するには、以下のコマンドを使用します。
* `chronyc sources -v` （同期ソースの一覧表示）
* `chronyc tracking` （現在の時刻誤差や統計情報の表示）


* **即時反映**:
設定変更後は `systemctl restart chronyd` で反映が必要ですが、急激な時刻変更（ステップ修正）を避けたい場合は慎重に実施してください。


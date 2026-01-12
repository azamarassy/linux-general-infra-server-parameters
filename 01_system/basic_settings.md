# システム構成定義・確認コマンド一覧

## 1. 仮想ハードウェア構成

| 項目 | 内容 | 取得コマンド例 |
| --- | --- | --- |
| 仮想CPU | 2vCPU | `lscpu` |
| メモリ | 3.6GiB | `free -h` |
| メモリスワップ | 2GB | `swapon -s`, `free -h` |
| HDD(基本)領域 | 48GB | `lsblk`, `df -h` |
| NIC | ensxx | `ifconfig` |

---

## 2. システム構成

| 項目 | 内容 | 取得コマンド例 |
| --- | --- | --- |
| ホスト名 | example | `hostname`, `hostnamectl` |
| システム名 | Rocky Linux | `cat /etc/os-release`, `hostnamectl` |
| カーネル | xxx(Rocky Linux 8.10).x86_64 | `uname -r` |
| システム標準言語 | ja_JP.UTF-8 | `localectl`, `echo $LANG` |
| キーボードレイアウト | jp | `localectl`, `localectl status` |
| ゾーン/グループ | Asia/Tokyo | `timedatectl` |
| サーバー起動ランレベル | 5 | `systemctl get-default` |

---

## 3. アップデート設定

| 項目 | 内容 | 取得コマンド例 |
| --- | --- | --- |
| 自動更新 | inactive | `systemctl status dnf-automatic.timer` |
| アップデート除外 | kernel* | `cat /etc/dnf/dnf.conf` |
| パッケージキャッシュ | keepcache=0 | `dnf config-manager --dump | grep keepcache` |
| プロキシ設定 | - | `grep proxy /etc/yum.conf` |

---

## 4. 時刻設定

| 項目 | 内容 | 取得コマンド例 |
| --- | --- | --- |
| タイムゾーン | Asia/Tokyo (JST, +0900) | `timedatectl` |
| インターネット時刻 | no | `timedatectl` |
| NTPサーバー設定 | inactive | `timedatectl` |

---

## 5. ディスク設定

### ディスク構成

| デバイス名 | 内容 | 取得コマンド例 |
| --- | --- | --- |
| /dev/sda | 基本ディスク | `lsblk`, `fdisk -l` |
| /dev/sdb | 追加ディスク | `lsblk`, `fdisk -l` |

### パーティション構成

> **取得コマンド例:** `lsblk`

| ファイルシステム | マウントポイント | 容量 | タイプ | 
| --- | --- | --- | --- | 
| /dev/sda | - | 50G | - | 
| /dev/sda1 | /boot | 1G | xfs |
| /dev/sda2 | /boot/efi | 1G | vfat |
| /dev/sda3 | / | 48G | LVM |
| /dev/mapper/rootvg-lvroot | / | 43G | xfs |
| /dev/mapper/rootvg-lvtmp | /tmp | 3G | xfs |
| /dev/mapper/rootvg-lvswap | swap | 2G | swap |
| /dev/sr0 | /run/media/general/Rocky-8-10-x86_64-dvd | 13.2G | iso9660 |

---

## 6. ネットワーク設定

### インターフェース設定

| NIC | IP | Network Address | Network Mask | Gateway | 取得コマンド例 |
| --- | --- | --- | --- | --- | --- |
| ens33 | 192.168.x.x | 192.168.x.x | 255.255.255.0 | 192.168.x.x | `ip route show` |

### スタティックルート

| ネットワークアドレス | ネットマスク | ゲートウェイアドレス | 備考 | 取得コマンド例 |
| --- | --- | --- | --- | --- |
| 192.168.x.x | 255.255.255.0 | 192.168.x.x | デフォルトゲートウェイ経由 | `ip route show` |

### DNS設定

| 項目 | IP | 取得コマンド例 |
| --- | --- | --- |
| プライマリ | 8.8.8.8 | `cat /etc/resolv.conf` |

---

## 7. 追加パッケージ

> **取得コマンド例:** `rpm -qa` または `dnf list installed`

* zabbix-agent
* rsync
* httpd
* libxml2
* libcurl
* strace
* logrotate
* firewalld

# ファイルシステムマウント設定 (`/etc/fstab`)

| マウントポイント | デバイス / UUID | ファイルシステムタイプ | オプション | ダンプフラグ | パス検査フラグ |
| --- | --- | --- | --- | --- | --- |
| `/` | `/dev/mapper/rootvg-lvroot` | `xfs` | `defaults` | 0 | 0 |
| `/boot` | `UUID=xxx` | `xfs` | `defaults` | 0 | 0 |
| `/boot/efi` | `UUID=xxx` | `vfat` | `umask=0077,shortname=winnt` | 0 | 2 |
| `/tmp` | `/dev/mapper/rootvg-lvtmp` | `xfs` | `defaults` | 0 | 0 |

---

### 設定内容

* **デバイス / UUID**: システムがディスクを識別するための識別子です。`/boot` などは起動の確実性を高めるため、デバイス名（/dev/sda1等）ではなく固有の **UUID** で指定しています。
* **オプション**:
* `defaults`: rw, suid, dev, exec, auto, nouser, async のセットです。
* `umask=0077`: EFIパーティションに対するパーミッション設定です。


* **パス検査フラグ (fsck)**:
* `0`: 起動時のファイルシステムチェックを行いません。
* `2`: ルートディレクトリ以外でチェックを行う際の優先度です。


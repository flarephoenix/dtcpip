# SeeQVault

## SeeQVault とは

SeeQVault は、Panasonic・Sony・Samsung・東芝の4社が共同策定した、**録画コンテンツのポータブルストレージ移動を可能にする DRM 規格**。2013年頃に策定・普及が始まった。

DTCP-IP がネットワーク上での配信を制御するのに対し、SeeQVault は **USB HDD・SD カードなどの物理メディアに暗号化したまま記録・移動** する仕組みを提供する。

---

## DTCP-IP と SeeQVault の違い

| 観点 | DTCP-IP | SeeQVault |
|---|---|---|
| 用途 | ホームネットワーク内での配信 | USB HDD・SD カードへの移動 |
| 方式 | ネットワーク（TCP/IP） | 物理ストレージ（USB/SD） |
| 対応デバイス | BD レコーダー、DLNAサーバー | BD レコーダー、テレビ、PC |
| PC 再生 | DiXiM Play 等が必要 | SeeQVault Player Plus 等が必要 |
| 暗号方式 | AES-128 + DTLA 証明書 | AES-128 + NSM（ナノセキュリティモジュール）|
| ライセンス管理 | DTLA | SeeQVault LLC |

---

## DTCP+ との混同に注意

「SeeQVault は DTCP+ だ」という誤解が散見されるが、両者は別物である。

| 規格 | 説明 |
|---|---|
| **DTCP-IP** | ホームネットワーク内でのコンテンツ共有（LAN 内のみ） |
| **DTCP+** | DTCP-IP の拡張版。インターネット越しのリモートアクセスに対応（自宅外からのストリーミング） |
| **SeeQVault** | USB HDD・SD カードへの物理的なコンテンツ移動 |

→ DTCP+ はあくまで「ネットワーク経由でのリモート視聴」を実現するもの。USB HDD への保存とは無関係。

---

## Sony BDZ-EW510 と SeeQVault

> **結論: BDZ-EW510 は SeeQVault に対応している。**

SeeQVault の公式デバイスリスト（[jp.seeqvault.com](https://jp.seeqvault.com/devicelist-pcmb.html)）に BDZ-EW510 が掲載されている。

ただし、SeeQVault 経由の保存・移動には **SeeQVault 対応の外付け USB HDD** が必要。

### 対応フロー

```
BDZ-EW510
    │
    │（SeeQVault ダビング）
    ▼
SeeQVault 対応 USB HDD（例: IO-DATA HDJA-UT シリーズ、Buffalo HD-AD シリーズ）
    │
    │（USB 接続または同一 LAN）
    ▼
PC（SeeQVault Player インストール済み）
    │
    └─ SeeQVault Player Plus（Sony 製）
    └─ DiXiM SeeQVault Play（DigiOn 製）
    └─ CyberLink SeeQVault Player（CyberLink 製）
```

### 手順

1. BDZ-EW510 に SeeQVault 対応 USB HDD を接続（または DLNA 経由でダビング）
2. BDZ-EW510 のダビングメニューから「外付けハードディスク（SeeQVault）」を選択
3. SeeQVault 形式でダビング（移動）
4. その USB HDD を PC に接続、または同じ LAN に置く
5. PC 側の SeeQVault 対応アプリで再生

---

## PC 対応の SeeQVault プレーヤー

| ソフト | 開発元 | 対応 OS | 価格 | 備考 |
|---|---|---|---|---|
| **SeeQVault Player Plus** | Sony | Windows | 有料（¥2,750 前後） | USB 接続か NAS 経由 |
| **DiXiM SeeQVault Play** | DigiOn | Windows | 有料 | DiXiM Play と同一エコシステム |
| **CyberLink SeeQVault Player** | CyberLink | Windows | 有料 | PowerDVD との連携 |

> ⚠️ Mac 対応の SeeQVault プレーヤーは現時点（2026年3月）では確認できていない。

---

## SeeQVault 対応 USB HDD

代表的な対応製品:

- **IO-DATA HDJA-UT シリーズ**（SeeQVault 対応明記）
- **Buffalo HD-AD シリーズ**（SeeQVault 対応明記）
- **Panasonic DY-HD1500V** など（放送録画対応モデル）

→ 接続する機器との相性確認が必要。公式互換リストを参照すること。

---

## SeeQVault の限界

| 制限 | 内容 |
|---|---|
| コピー制限 | ダビング10に従う（コピー回数制限あり） |
| 互換性 | SeeQVault 対応デバイス間でのみ再生可能 |
| 形式変換 | SeeQVault 保護のまま。MP4 等への変換は不可 |
| 将来性 | 規格自体は継続中だが、BD レコーダー市場縮小により普及デバイスが減少傾向 |

---

## BDZ-EW510 ユーザーへの推奨

SeeQVault 経由の保存は、**DTCP-IP よりも実現可能性が高い**代替手段の一つ。

ただし:
- SeeQVault 対応 USB HDD の購入が必要（数千〜数万円）
- SeeQVault 対応プレーヤーの購入が必要（¥2,000〜3,000）
- コンテンツは SeeQVault 暗号化のまま残るため、自由なファイル操作は不可

**→ IO-DATA BDレコ（BDRC-APP）や DiXiM Play と組み合わせて検討するのが現実的。**

---

## 参考リンク

- [SeeQVault 公式 デバイスリスト（PC/モバイル）](https://jp.seeqvault.com/devicelist-pcmb.html)
- [SeeQVault 公式サイト](https://jp.seeqvault.com/)
- [DiXiM SeeQVault Play](https://www.digion.com/sites/diximplay/seeqvault/)

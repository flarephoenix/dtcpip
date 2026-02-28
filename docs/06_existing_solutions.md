# 既存の合法的な解決策

## 比較一覧

| ソフト | 対応 OS | 費用 | BDZ-EW510 対応 | 用途 |
|---|---|---|---|---|
| **IO-DATA BDレコ（BDRC-APP）** | Windows | ¥3,278 | 要確認 | BD/DVD ディスクへのダビング |
| **DiXiM Digital TV / DiXiM Play** | Win / Mac / iOS / Android | ¥1,430〜2,860 | ○（対応確認済み） | ネットワーク視聴・録画管理 |
| **PC TV Plus** | Windows | ¥4,400 | ○ | ネットワーク視聴・録画管理 |
| **RECBOX 経由** | — | NAS 本体代 | ○（ダビング対応） | NAS への保存（保護維持） |

---

## IO-DATA BDレコ（BDRC-APP）

**発表**: 2026年2月25日　**発売**: 2026年2月末
**入手先**: Microsoft Store
**公式**: https://www.iodata.jp/news/2026/newprod/bdrc-app.htm

### 特徴

- PC に接続した **BD/DVD ドライブへ録画番組をダビング** するためのアプリ
- 「ネットワークダビング」対応機器からコンテンツを受信（内部的に DTCP-IP を使用）
- **BD/DVD ドライブが PC に必要**（外付けで可）
- 操作モードが2種類:
  - **TV 操作モード**: テレビの画面とリモコンで番組選択
  - **PC 操作モード**: PC の画面で番組一覧を確認・選択

### 対応機器（抜粋）

- Sharp AQUOS テレビ
- Panasonic テレビ（Fire TV モデルを除く）
- Toshiba REGZA
- ケーブル・衛星チューナー（ひかり TV、スカパー等）
- nasne / miyotto

**BDZ-EW510 との互換性**: 「ネットワークダビング対応機器」が対象だが、BDZ-EW510 は同機能を持つため対応している可能性あり。**要実機確認。**

### 制限事項

- Windows のみ（Mac 非対応）
- ディスクへのダビングが目的のため、PC の HDD への保存は非対応（の可能性）
- コンテンツは DTCP-IP 保護のまま扱われる

---

## DiXiM Digital TV / DiXiM Play

**開発元**: 株式会社ダイジオン（DigiOn, Inc.）
**公式**: https://www.digion.com/product/dixim-digital-tv/

### 特徴

- DTLA 正規ライセンスを取得した DTCP-IP クライアント
- BDZ-EW510 との**互換性が確認されている**（DiXiM Digital TV 2013 以降）
- 視聴・録画予約管理が可能
- 外出先からのリモート視聴機能あり（製品による）

### 価格（2025年〜）

| 製品 | 価格 | 備考 |
|---|---|---|
| DiXiM Play（Windows） | ¥2,860 | 一般版 |
| DiXiM Play U（Windows） | ¥1,430 | プロモーション価格あり |
| DiXiM Play（iOS / Android） | ¥1,430 | 月額 ¥220 も選択可 |
| DiXiM Play（Mac） | ¥1,430 | Apple Silicon のみ対応、Mac mini 等は非対応 |

### 制限事項

- Windows 版は **Intel CPU のみ**（AMD GPU/APU での動作保証なし）
- Mac 版は **Apple Silicon 搭載 Mac のみ**（Mac mini、ディスプレイなし Mac は非対応）
- 再インストール回数に制限がある場合がある

---

## PC TV Plus（Sony 公式）

**開発元**: ソニーマーケティング株式会社
**公式**: https://www.sony.jp/software/store/products/pctv-plus/

### 特徴

- BDZ シリーズとの連携を前提とした Sony 公式ソフト
- BDZ-EW510 との互換性あり
- 視聴・録画予約・Blu-ray 書き出しが可能

### 価格

| 製品 | 価格 |
|---|---|
| PC TV Plus | ¥4,400 |
| PC TV Plus Lite | ¥3,300 |

### 注意事項

- **外部録画機能（PC TV Plus 経由）は 2027年7月にサービス終了予定**
- Windows のみ
- Sony BDZ との組み合わせを前提としているため、汎用性は低い

---

## IO-DATA RECBOX 経由

### 仕組み

```
BDZ-EW510 → (DTCP-IP ダビング) → RECBOX（NAS）→ PC で DiXiM を使って視聴
```

BDZ-EW510 は RECBOX への自動ダビングに対応している。

### 重要な制限

- **RECBOX に保存した後もコンテンツは DTCP-IP 保護のまま**
- PC から「自由に」アクセスできるわけではなく、DTCP-IP 対応ソフト（DiXiM 等）が必要
- 4K コンテンツは RECBOX にダビング不可
- コンテンツは「解放」されるわけではない → あくまで保存場所が変わるだけ

### メリット

- BDZ-EW510 がサポート終了してもコンテンツは RECBOX に残る
- 家庭内の複数デバイスから視聴可能になる

---

## 参考リンク

- [IO-DATA BDレコ 公式](https://www.iodata.jp/product/storage/blu-ray/bdrc-app/)
- [DiXiM Play Windows 版](https://www.digion.com/sites/diximplay/windows-u/)
- [PC TV Plus 公式](https://www.sony.jp/software/store/products/pctv-plus/)
- [IO-DATA DTCP-IP 対応機器一覧](https://www.iodata.jp/pio/io/hdd/dtcpip.htm)

# 調査ノート（詳細版）

> **README.md** が概要とエントリポイントです。まず [README.md](README.md) を参照してください。
> このファイルは調査過程で記録した詳細な一次ノートです。

---

## 対象機器: Sony BDZ-EW510

→ 詳細: [docs/01_device_bdz_ew510.md](docs/01_device_bdz_ew510.md)

| 項目 | 内容 |
|---|---|
| 発売時期 | 2013年頃 |
| DLNA | 対応（メディアサーバー） |
| DTCP-IP | 対応 |
| ネットワークムーブ | **非対応** |
| RECBOX 自動ダビング | 対応 |

---

## DTCP-IP

→ 概要: [docs/02_dtcp_ip_overview.md](docs/02_dtcp_ip_overview.md)
→ 技術詳細: [docs/03_dtcp_ip_technical.md](docs/03_dtcp_ip_technical.md)
→ ライセンス: [docs/04_dtcp_ip_licensing.md](docs/04_dtcp_ip_licensing.md)

### ライセンス費用（推測）

DTLA は金額を非公開にしている。状況証拠から **年間数十万〜数百万円規模**と推測。
個人向けライセンスパスは存在しない。

### OSS の状況

GStreamer プラグイン（[lyrato/gst-plugins-dtcpip](https://github.com/lyrato/gst-plugins-dtcpip)）は「殻」のみ。
コア DLL はプロプライエタリで非公開。Python/Rust の完全実装は存在しない。

---

## 法的側面

→ 詳細: [docs/05_japanese_copyright_law.md](docs/05_japanese_copyright_law.md)

- 著作権法第30条: 個人利用は原則 OK
- 著作権法第30条第2項: **技術的保護手段の回避は私的使用でも違法**
- DTCP-IP は技術的保護手段に該当する可能性が高い
- 個人利用・非配布での自作は民事上グレーだが刑事罰リスクは低い

---

## 既存の合法的な解決策

→ 詳細: [docs/06_existing_solutions.md](docs/06_existing_solutions.md)

| ソフト | 費用 | BDZ-EW510 |
|---|---|---|
| **IO-DATA BDレコ（BDRC-APP）** | ¥3,278 | 要確認 |
| DiXiM Play（Windows） | ¥2,860 | ○ |
| PC TV Plus | ¥4,400 | ○ |

---

## 代替手段

→ 詳細: [docs/07_alternatives.md](docs/07_alternatives.md)

- **HDMI キャプチャー**: 等速・若干の画質劣化あり。法的にはグレー
- **RECBOX 経由**: コンテンツは DTCP-IP 保護のまま。PC 視聴には DiXiM 必要
- **nasne**: Sony 版は 2027年7月サービス終了。Buffalo 版は継続中

---

## 自作アプリ（断念）

→ 詳細: [docs/08_diy_app_architecture.md](docs/08_diy_app_architecture.md)

技術スタック: C# .NET 8 + WPF + Rssdp
DTCP-IP 部分: `IDtcpipProvider` インターフェースで外部 DLL を P/Invoke

**DTLA 証明書の入手が不可能なため、断念。**

---

## 録画文化の衰退

→ 詳細: [docs/09_recording_culture_decline.md](docs/09_recording_culture_decline.md)

- Sony が 2026年2月に BD レコーダー事業撤退（最終出荷完了）
- TVS REGZA（東芝）も 2026年1月に撤退（船井電機破産が引き金）
- ストリーミングが録画を代替
- コピー制限強化が皮肉にもストリーミング移行を加速させた

---

## SeeQVault

→ 詳細: [docs/10_seeqvault.md](docs/10_seeqvault.md)

- **BDZ-EW510 は SeeQVault に対応している**（公式デバイスリスト掲載）
- SeeQVault = USB HDD / SD カードへの暗号化移動（DTCP-IP はネットワーク配信）
- DTCP+ はネットワーク経由のリモートアクセス拡張。SeeQVault とは別物
- PC 再生: SeeQVault Player Plus（Sony）、DiXiM SeeQVault Play（DigiOn）

---

## 著作権法の国際比較

→ 詳細: [docs/11_copyright_international.md](docs/11_copyright_international.md)

- 日本・米国（DMCA §1201）は最も厳格: 私的使用でも技術的保護手段の回避は違法
- EU・カナダは若干緩いが DTCP-IP 回避が合法とはいえない
- オーストラリアはタイムシフト合法化済み（2006年改正）だが回避は別問題

---

## 実践ガイド

→ 詳細: [docs/12_practical_guide.md](docs/12_practical_guide.md)

- **BDZ-EW510 は2013年製で HDD が約12〜13年経過（設計寿命超）。今すぐ救出を**
- M2TS → MP4 変換: `ffmpeg -i input.m2ts -vcodec copy -acodec copy output.mp4`（ロスレス）
- Wireshark の DTCP-IP ディセクタ: `packet-dtcp-ip.c`（AKE フローは観測可能、コンテンツは暗号化）
- Video & TV SideView（Sony スマホアプリ）は **2027年3月30日サービス終了**

---

## Blu-ray vs HD DVD フォーマット戦争

→ 詳細: [docs/13_format_war.md](docs/13_format_war.md)

- 2006〜2008年: Blu-ray（Sony 主導）と HD DVD（東芝主導）が競合
- PS3 の Blu-ray 搭載が決定打。Warner Bros. の BD 支持で 2008年2月に東芝が撤退
- 皮肉: Blu-ray 戦争に勝利した Sony 自身が 2026年2月に BD レコーダー撤退

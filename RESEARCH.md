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

- Sony が 2025年に BD レコーダー事業撤退
- ストリーミングが録画を代替
- コピー制限強化が皮肉にもストリーミング移行を加速させた

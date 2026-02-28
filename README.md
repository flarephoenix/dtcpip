# DTCP-IP 録画救出 調査レポート

> Sony BDZ-EW510（サポート終了）に録画された番組を PC で取り出せるか調査したレポートです。
> 同じ問題に直面している方の参考になれば。

## TL;DR（結論）

| やりたいこと | 結論 |
|---|---|
| PC で録画を再生したい | **DiXiM Play**（¥2,860）を使う |
| BD/DVD ディスクに保存したい | **IO-DATA BDレコ**（¥3,278）を使う |
| SeeQVault 対応 HDD に移したい | **SeeQVault Player Plus** 等で対応可（BDZ-EW510 対応確認済み） |
| 自作アプリを作りたい | **現実的に困難**（DTLA ライセンスが必要） |
| 完全に無料で解決したい | **難しい**（法的にも技術的にも） |

**⚠️ BDZ-EW510 は 2013年製で HDD が約12〜13年経過（設計寿命超）。今すぐ行動を推奨。**

---

## 免責事項

- 本レポートは個人の調査記録です。法的・技術的な正確性を保証しません
- DTCP-IP の実装・回避に関わる情報は教育・研究目的の記録です
- 著作権法に関わる行為を行う際は、自己責任で専門家に相談してください
- 本レポートの情報を使用した結果について、筆者は責任を負いません

---

## 背景

Sony BDZ-EW510（2013年製 BD/HDD レコーダー）がサポート終了となり、内部に録画された番組を救い出す方法を探した。

レコーダーは **DTCP-IP** に対応しており、理論上は PC からネットワーク経由でコンテンツを取り出せるはず…だが、DTCP-IP には強力な DRM がかかっており、実現には様々な障壁があった。

---

## ドキュメント一覧と要点

### 機器・プロトコル

| ファイル | 内容 |
|---|---|
| [docs/01_device_bdz_ew510.md](docs/01_device_bdz_ew510.md) | Sony BDZ-EW510 の仕様・DLNA 詳細 |
| [docs/02_dtcp_ip_overview.md](docs/02_dtcp_ip_overview.md) | DTCP-IP とは何か（概要） |
| [docs/03_dtcp_ip_technical.md](docs/03_dtcp_ip_technical.md) | DTCP-IP の技術詳細（AKE・PCP・HTTP ヘッダー） |

**主要ポイント:**
- BDZ-EW510: DLNA / DTCP-IP 対応。**ネットワークムーブは非対応**。RECBOX への自動ダビングは対応
- DTCP-IP ライセンス: DTLA が管理。年間**数十万〜数百万円規模**（推測）。個人向けパスは存在しない
- OSS 実装なし: [gst-plugins-dtcpip](https://github.com/lyrato/gst-plugins-dtcpip) は「殻」のみ。コア DLL は非公開。Python / Rust の完全実装も存在しない

### ライセンス・法律

| ファイル | 内容 |
|---|---|
| [docs/04_dtcp_ip_licensing.md](docs/04_dtcp_ip_licensing.md) | DTLA ライセンス構造と費用（推測含む） |
| [docs/05_japanese_copyright_law.md](docs/05_japanese_copyright_law.md) | 日本の著作権法と DTCP-IP の関係 |
| [docs/11_copyright_international.md](docs/11_copyright_international.md) | 著作権法の国際比較（日本・米国・EU・カナダ等） |

**主要ポイント:**
- 著作権法第30条: 個人利用目的の複製は原則 OK
- 著作権法第30条の2: **技術的保護手段の回避は私的使用目的でも違法**
- DTCP-IP は「技術的保護手段」に該当する可能性が高い。個人利用・非配布での自作はグレーだが刑事罰リスクは低い
- 国際比較: 日本・米国（DMCA §1201）が最も厳格。EU・カナダは若干緩いが DTCP-IP 回避が合法とはいえない

### 解決策

| ファイル | 内容 |
|---|---|
| [docs/06_existing_solutions.md](docs/06_existing_solutions.md) | 既存の合法的な解決策（BDレコ・DiXiM・PC TV Plus・RECBOX） |
| [docs/07_alternatives.md](docs/07_alternatives.md) | 代替手段（HDMI キャプチャー・nasne・BD メディア） |
| [docs/08_diy_app_architecture.md](docs/08_diy_app_architecture.md) | 自作アプリのアーキテクチャ設計（断念した経緯と参考） |
| [docs/10_seeqvault.md](docs/10_seeqvault.md) | SeeQVault（BDZ-EW510 対応確認）と DTCP+ との違い |
| [docs/12_practical_guide.md](docs/12_practical_guide.md) | 実践ガイド（HDD 寿命・ffmpeg・Wireshark・スマホアプリ） |

**既存ソフト比較:**

| ソフト | 費用 | BDZ-EW510 | 備考 |
|---|---|---|---|
| **IO-DATA BDレコ（BDRC-APP）** | ¥3,278 | 要互換確認 | BD/DVD ディスクへダビング。Windows のみ |
| **DiXiM Play（Windows）** | ¥2,860 | **○ 確認済み** | PC からの視聴・管理。Mac 版もあり |
| **PC TV Plus（Sony）** | ¥4,400 | ○ | 2027年3月サービス終了 |
| **SeeQVault Player Plus** | ¥2,750 前後 | **○ 確認済み** | SeeQVault 対応 USB HDD が別途必要 |

**主要ポイント:**
- **BDZ-EW510 は SeeQVault に対応**（公式デバイスリスト掲載）。SeeQVault 対応 USB HDD を使えば暗号化したまま移動し、PC で再生できる
- HDMI キャプチャーは等速・若干の画質劣化あり。法的にはグレー
- nasne: Sony 版は 2027年7月サービス終了。Buffalo 版は継続中
- 自作アプリは DTLA 証明書の入手が不可能なため断念（技術スタック: C# .NET 8 + WPF + Rssdp）

### 考察・補足

| ファイル | 内容 |
|---|---|
| [docs/09_recording_culture_decline.md](docs/09_recording_culture_decline.md) | 録画機器・録画文化の衰退についての考察 |
| [docs/13_format_war.md](docs/13_format_war.md) | Blu-ray vs HD DVD フォーマット戦争の歴史 |

**主要ポイント:**
- Sony（2026年2月）・TVS REGZA＝東芝（2026年1月）が BD レコーダー市場から撤退。現在継続は Panasonic・Sharp（OEM）のみ
- 市場ピーク（2011年 約639万台）→ 2025年 約62万台（10分の1 以下）
- コピー制限強化が皮肉にもストリーミング移行を加速させ、録画市場を自滅させた
- Blu-ray は 2008年の HD DVD との規格戦争に勝利（PS3 搭載が決定打）したが、18年後に Sony 自身が撤退という歴史の皮肉

---

## 推奨アクション順序

```
Step 0（最優先）: HDD の健康状態を確認
         └─ 異音・フリーズ・再生失敗があれば即座に救出開始
         └─ BDZ-EW510 は2013年製。HDD は設計寿命を超えている

Step 1: IO-DATA BDレコ（BDRC-APP）を試す
         └─ 互換リスト: https://www.iodata.jp/pio/io/storage/bdrec.htm
         └─ 対応していれば BD/DVD ディスクに保存できる

Step 2: DiXiM Play（Windows/Mac）を試す
         └─ BDZ-EW510 との互換性が確認済み
         └─ PC からの視聴・管理が可能

Step 3: SeeQVault 経由で USB HDD に移動
         └─ SeeQVault 対応 USB HDD（IO-DATA / Buffalo 等）を別途購入
         └─ BDZ-EW510 → USB HDD → SeeQVault Player Plus で PC 再生

Step 4: RECBOX を経由する
         └─ BDZ → RECBOX → DiXiM で視聴
         └─ コンテンツは DTCP-IP 保護のまま

Step 5（最終手段）: HDMI キャプチャー
         └─ 再生しながら画面録画
         └─ 若干の画質劣化あり・等速でしか録画できない
```

---

## 調査を通じて分かったこと

1. **DTCP-IP は思っていた以上に強固な DRM だった**
   OSS 実装が一切存在せず、ライセンス費用も非公開で個人には手が出ない。自作の唯一の壁は DTLA 証明書の一点。

2. **BDZ-EW510 は SeeQVault に対応している**
   見落とされがちだが、SeeQVault 対応 USB HDD を使えば DTCP-IP を介さずにコンテンツを移動できる。

3. **法的にはどの国でも DTCP-IP 回避は基本違法**
   日本・米国が特に厳格。個人利用目的でも「技術的保護手段の回避」に該当する可能性がある。

4. **HDD の寿命が深刻**
   BDZ-EW510 の HDD は約12〜13年が経過し、統計的に故障率が急増する時期を過ぎている。

5. **録画文化自体が終わりに近づいている**
   Sony・東芝がレコーダー事業から撤退。ストリーミングの台頭とコピー制限強化が皮肉にも録画文化を自滅させた。

---

## 調査時期

2026年3月

---

## 参考リンク

- [DTLA 公式](https://www.dtcp.com/)
- [IO-DATA BDレコ 発表](https://www.iodata.jp/news/2026/newprod/bdrc-app.htm)
- [IO-DATA BDレコ 互換機器リスト](https://www.iodata.jp/pio/io/storage/bdrec.htm)
- [DiXiM Play（Windows）](https://www.digion.com/sites/diximplay/windows-u/)
- [SeeQVault 公式 デバイスリスト](https://jp.seeqvault.com/devicelist-pcmb.html)
- [Sony BDZ-EW510（アーカイブ）](https://www.sony.jp/bd/products/BDZ-EW510/)
- [gst-plugins-dtcpip（OSS の DTCP-IP ラッパー）](https://github.com/lyrato/gst-plugins-dtcpip)

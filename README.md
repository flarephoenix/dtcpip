# DTCP-IP 録画救出 調査レポート

> Sony BDZ-EW510（サポート終了）に録画された番組を PC で取り出せるか調査したレポートです。
> 同じ問題に直面している方の参考になれば。

## TL;DR（結論）

| やりたいこと | 結論 |
|---|---|
| PC で録画を再生したい | **DiXiM Play**（¥2,860）を使う |
| BD/DVD ディスクに保存したい | **IO-DATA BDレコ**（¥3,278）を使う |
| 自作アプリを作りたい | **現実的に困難**（DTLA ライセンスが必要） |
| 完全に無料で解決したい | **難しい**（法的にも技術的にも） |

**Sony BDZ-EW510 は DiXiM Digital TV と互換性が確認されています（2013年版以降）。**

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

## ドキュメント一覧

### 機器・プロトコル

| ファイル | 内容 |
|---|---|
| [docs/01_device_bdz_ew510.md](docs/01_device_bdz_ew510.md) | Sony BDZ-EW510 の仕様・DLNA 詳細 |
| [docs/02_dtcp_ip_overview.md](docs/02_dtcp_ip_overview.md) | DTCP-IP とは何か（概要） |
| [docs/03_dtcp_ip_technical.md](docs/03_dtcp_ip_technical.md) | DTCP-IP の技術詳細（AKE・PCP・HTTP ヘッダー） |

### ライセンス・法律

| ファイル | 内容 |
|---|---|
| [docs/04_dtcp_ip_licensing.md](docs/04_dtcp_ip_licensing.md) | DTLA ライセンス構造と費用（推測含む） |
| [docs/05_japanese_copyright_law.md](docs/05_japanese_copyright_law.md) | 日本の著作権法と DTCP-IP の関係 |

### 解決策

| ファイル | 内容 |
|---|---|
| [docs/06_existing_solutions.md](docs/06_existing_solutions.md) | 既存の合法的な解決策（BDレコ・DiXiM・PC TV Plus・RECBOX） |
| [docs/07_alternatives.md](docs/07_alternatives.md) | 代替手段（HDMI キャプチャー・nasne・BD メディア） |
| [docs/08_diy_app_architecture.md](docs/08_diy_app_architecture.md) | 自作アプリを作る場合のアーキテクチャ設計（参考） |

### 考察・補足

| ファイル | 内容 |
|---|---|
| [docs/09_recording_culture_decline.md](docs/09_recording_culture_decline.md) | 録画機器・録画文化の衰退についての考察 |
| [docs/10_seeqvault.md](docs/10_seeqvault.md) | SeeQVault（BDZ-EW510 対応確認）と DTCP+ との違い |
| [docs/11_copyright_international.md](docs/11_copyright_international.md) | 著作権法の国際比較（日本・米国・EU・カナダ等） |
| [docs/12_practical_guide.md](docs/12_practical_guide.md) | 実践ガイド（HDD 寿命・ffmpeg・Wireshark・スマホアプリ） |
| [docs/13_format_war.md](docs/13_format_war.md) | Blu-ray vs HD DVD フォーマット戦争の歴史 |

---

## 推奨アクション順序

```
Step 1: IO-DATA BDレコ（BDRC-APP）を試す
         └─ BDZ-EW510 が対応していれば BD/DVD ディスクに保存できる

Step 2: DiXiM Play（Windows/Mac）を試す
         └─ BDZ-EW510 との互換性が確認済み
         └─ PC からの視聴・管理が可能

Step 3: RECBOX を経由する
         └─ BDZ → RECBOX → DiXiM で視聴
         └─ コンテンツは DTCP-IP 保護のまま

Step 4（最終手段）: HDMI キャプチャー
         └─ 再生しながら画面録画
         └─ 若干の画質劣化あり
         └─ 等速でしか録画できない
```

---

## 調査を通じて分かったこと

1. **DTCP-IP は思っていた以上に強固な DRM だった**
   OSS 実装が一切存在せず、ライセンス費用も非公開で個人には手が出ない。

2. **自作アプリの技術的な壁は DTLA 証明書の一点**
   プロトコルの仕組みはある程度公開されており、UPnP/DLNA 部分は実装可能。しかし AKE（認証）に DTLA 証明書が必須で、これなしには動作しない。

3. **BDZ-EW510 は SeeQVault に対応している**
   SeeQVault 対応 USB HDD を使えば、暗号化したまま外付けドライブに移動し、SeeQVault Player Plus 等で PC 再生できる。DTCP-IP の代替手段として有効。

4. **録画文化自体が終わりに近づいている**
   Sony・TVS REGZA（東芝）がレコーダー事業から撤退。ストリーミングが録画を代替しつつある。BDZ-EW510 の問題は、より大きな「デジタルコンテンツの保存」という問題の一部。

5. **HDD の寿命は設計の2倍以上が経過している**
   BDZ-EW510（2013年製）の HDD は約12〜13年が経過。故障リスクが高く、**今すぐ** コンテンツ救出を行動することを強く推奨。

6. **IO-DATA BDレコ（2026年）が現時点の最善策**
   既製品として合法的に使えるものがある。まずこれを試すのが正解。

---

## 調査時期

2026年3月

---

## 参考リンク

- [DTLA 公式](https://www.dtcp.com/)
- [IO-DATA BDレコ 発表](https://www.iodata.jp/news/2026/newprod/bdrc-app.htm)
- [DiXiM Play（Windows）](https://www.digion.com/sites/diximplay/windows-u/)
- [Sony BDZ-EW510（アーカイブ）](https://www.sony.jp/bd/products/BDZ-EW510/)
- [gst-plugins-dtcpip（OSS の DTCP-IP ラッパー）](https://github.com/lyrato/gst-plugins-dtcpip)

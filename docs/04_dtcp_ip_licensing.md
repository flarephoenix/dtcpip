# DTCP-IP ライセンス

## DTLA（Digital Transmission Licensing Administrator）

DTLA は DTCP-IP の知的財産を管理する団体。

- 設立企業: Sony、Intel、Hitachi、Matsushita（Panasonic）、Toshiba
- 本拠地: 米国カリフォルニア州
- 公式サイト: https://www.dtcp.com/

## ライセンスの種類

### Adopter Agreement（採用者契約）

DTCP-IP を製品に実装する企業・個人が結ぶ契約。

| カテゴリ | 概要 |
|---|---|
| **Small Adopter** | 年間管理費が低い、証明書取得コストが高い。少量生産メーカー向け |
| **Large Adopter** | 年間管理費が高い、証明書取得コストが低い。大量生産メーカー向け |
| **Evaluator** | より低い費用でプロトタイプ作成可（ダミー証明書使用）。製品出荷不可 |

**個人開発者向けのライセンスパスは存在しない。**

### ライセンスで得られるもの

1. DTCP-IP の完全仕様書（現在は一般公開版のみ・実装詳細は非公開）
2. DTLA 発行の証明書（AKE に必須）
3. テクニカルサポート

## 費用（推測・状況証拠レベル）

**DTLA は金額を非公開にしている。** 以下は状況証拠からの推測。

### 類似 DRM との比較

| DRM | 初期費用（概算） | 年間費用（概算） | 情報の確かさ |
|---|---|---|---|
| HDCP（Digital Content Protection LLC） | 約 $15,000 | 数千ドル規模 | 業界で比較的知られている |
| AACS（Blu-ray） | 数万ドル規模 | 数万ドル規模 | 非公開だが業界通説 |
| **DTCP-IP（推測）** | **不明** | **数十万〜数百万円規模と推測** | 推測 |

### 推測の根拠

1. **同世代 DRM の比較**: HDCP 等が $10,000〜$20,000/年 規模。DTCP-IP も同等以上と考えられる
2. **専業ベンダーの存在**: DiXiM（DigiOn）、Ubiquitous DTCP 等の中間ベンダーが DTLA と契約し、SDK を二次販売している。中間ベンダーが成立するということは、元ライセンスがそれなりに高額であることを示す
3. **エンドユーザー製品の価格**: DiXiM Digital TV が ¥1,430〜¥2,860。これがボリュームで採算が合う水準なら、元コストは数百万円規模が妥当
4. **「個人では払えない」という共通認識**: 業界関係者やエンジニアの間でこの認識が共有されている

### 結論

趣味・個人プロジェクトで DTLA に直接ライセンス申請するのは現実的でない。

## 商用 SDK（中間ベンダー）

DTLA から直接ライセンスを受けたベンダーが SDK を販売している。

| ベンダー | SDK 名 | 対応 OS |
|---|---|---|
| DigiOn（ダイジオン） | DiXiM SDK | Windows / Mac / Android / iOS |
| Ubiquitous AI Corporation | Ubiquitous DTCP | 組み込み Linux / RTOS |
| Morega Systems | DTCP-IP SDK | Android / iOS / Windows / Mac |
| Allegro Software | RomPlug DTCP-IP | 組み込み向け |
| Lynx Technology | Twonky SDK | マルチプラットフォーム |

これらも**問い合わせベースの見積もり**であり、価格は公開されていない。主に企業・メーカー向け。

## 仕様書の公開状況

- **情報提供版（Informational）**: DTLA 公式サイトから入手可能（[specifications.aspx](https://www.dtcp.com/specifications.aspx)）。ただし実装に必要な詳細（証明書形式・暗号パラメータ）は省略されている
- **完全版**: Adopter Agreement を締結した企業のみ入手可能

## 参考リンク

- [DTLA 公式](https://www.dtcp.com/)
- [DTLA ライセンス申請](https://www.dtcp.com/agreements.aspx)
- [DTLA FAQ](https://www.dtcp.com/faq.aspx)
- [DiXiM SDK（DigiOn）](https://www.digion.com/product/sdk/)
- [Ubiquitous DTCP](https://www.ubiquitous-ai.com/products/dtcp/)

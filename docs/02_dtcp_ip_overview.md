# DTCP-IP 概要

## DTCP-IP とは

**DTCP-IP（Digital Transmission Content Protection over Internet Protocol）** は、デジタル放送コンテンツをホームネットワーク（LAN）上で安全に共有するための著作権保護技術。

- 管理団体: **DTLA（Digital Transmission Licensing Administrator）**
  - 構成企業: Sony、Intel、Hitachi、Matsushita（Panasonic）、Toshiba の5社が設立
- 日本のテレビ録画機器に広く採用（2000年代〜）
- DLNA と組み合わせて使われることが多い

## 主な用途

| 用途 | 例 |
|---|---|
| 録画番組の宅内配信 | BD レコーダー → テレビ / スマートフォン |
| ダビング | BD レコーダー → RECBOX（NAS） |
| PC 視聴 | BD レコーダー → PC（DiXiM 等） |

## 通信の流れ

```
[クライアント（Sink）]              [レコーダー（Source）]
        |                                    |
        |── UPnP/SSDP でデバイス探索 ──────>|
        |<─ デバイス発見 ─────────────────── |
        |                                    |
        |── DLNA CDS Browse（SOAP）─────────>|
        |<─ 録画一覧（DIDL-Lite XML）─────── |
        |                                    |
        |── DTCP-IP AKE（認証・鍵交換）─────>|  ← ここが肝
        |<─ セッション鍵を共有 ──────────── |
        |                                    |
        |── HTTP GET（コンテンツ取得）───────>|
        |<─ AES-128 暗号化ストリーム（PCP）─ |
        |                                    |
        |── 復号 → 保存 / 再生              |
```

## DTCP-IP の保護レベル（E-EMI）

コンテンツごとにコピー制御情報が付与される:

| E-EMI 値 | 意味 |
|---|---|
| 0b00 | Copy Free（コピー自由） |
| 0b01 | 予約 |
| 0b10 | Copy One Generation（1世代コピー可：ダビング10等） |
| 0b11 | Copy Never（コピー不可） |

日本の地上デジタル放送の大半は「Copy One Generation」（ダビング10対象）。

## DTCP vs DTCP-IP vs DTCP2

| 規格 | 伝送路 | 特徴 |
|---|---|---|
| DTCP | IEEE 1394（FireWire） | 旧世代、現在はほぼ使われない |
| DTCP-IP | LAN（IP ネットワーク） | 現在主流 |
| DTCP2 | LAN | 2017年〜。4K/8K/HDR 対応。DTCP-IP とは非互換 |

## ダビング10 との関係

- 2008年6月より「ダビング10」制度が導入（地上デジタル放送）
- BD/DVD メディアへ最大9回コピー、10回目は自動的に「ムーブ（移動）」に変わる
- ネットワーク経由の DTCP-IP ダビングもこの制限に従う
- WOWOW・スカパー等のCS放送は「コピーワンス」のまま（1回しかコピーできない）

## 参考リンク

- [DTLA 公式](https://www.dtcp.com/)
- [DTCP-IP - Wikipedia](https://en.wikipedia.org/wiki/Digital_Transmission_Content_Protection)
- [DTCP-IP とは - e-Words](https://e-words.jp/w/DTCP-IP.html)

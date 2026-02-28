# 実践ガイド

> BDZ-EW510 からコンテンツを取り出すための実践的な情報をまとめる。
> ファイル変換、スマホアプリ、ハードウェアの寿命警告など。

---

## ⚠️ 緊急性について

**BDZ-EW510 は 2013年製のため、2026年3月時点で約12〜13年が経過している。**

### HDD の寿命

| 指標 | 値 |
|---|---|
| HDD の平均寿命（一般） | 3〜5年（連続稼働）/ 5〜7年（一般使用） |
| BDZ-EW510 の経過年数 | **約 12〜13年（設計寿命の約2倍）** |
| 故障率（10年超） | 統計的に急増（Backblaze データより） |

**→ いつ壊れてもおかしくない。コンテンツ救出は「今すぐ」行動することを強く推奨。**

### HDD 故障の前兆サイン

以下の症状が出たら、内蔵 HDD の故障が近い可能性が高い:

- 録画・再生時に**カシャカシャ・コツコツ**という異音がする（クリック音 = "Click of Death"）
- 再生中に**突然フリーズ・シャットダウン**する
- **録画予約の失敗**が増える
- **特定の番組が再生できない**（読み取りエラー）
- 本体が**通常より熱い**
- 起動に**異常に時間がかかる**
- 操作への**レスポンスが遅い**

---

## M2TS ファイルの扱い方

BD レコーダーから（DTCP-IP 保護を解除した合法的な手段で）取り出したコンテンツは、`.m2ts` 形式（BDAV MPEG-2 TS）で保存される。

### M2TS の特徴

| 項目 | 内容 |
|---|---|
| 拡張子 | `.m2ts` / `.mts` |
| コンテナ | BDAV MPEG-2 Transport Stream |
| パケットサイズ | **192バイト**（通常 TS の 188バイト + 4バイトのタイムスタンプ） |
| 映像コーデック | MPEG-2 または H.264/AVC |
| 音声コーデック | AAC / Dolby Digital / DTS |

### 再生

`.m2ts` ファイルはそのままメジャーなプレーヤーで再生できる:

- **VLC** (無料)
- **mpv** (無料)
- **MPC-HC** / **MPC-BE** (無料, Windows)
- **PowerDVD** (有料)

### ffmpeg による変換

```bash
# MP4 へのコンテナ変換（コーデックはそのまま・無劣化）
ffmpeg -i input.m2ts -vcodec copy -acodec copy -f mp4 output.mp4

# MP4 へ変換（音声を AAC に再エンコード）
ffmpeg -i input.m2ts -vcodec copy -acodec aac output.mp4

# H.264 動画として再エンコード（ファイルサイズ削減、若干の画質低下）
ffmpeg -i input.m2ts -vcodec libx264 -crf 18 -acodec aac output.mp4

# MKV へ変換（字幕トラックも保持）
ffmpeg -i input.m2ts -vcodec copy -acodec copy output.mkv

# 映像・音声コーデックの確認
ffprobe -v quiet -print_format json -show_streams input.m2ts
```

> 💡 `-vcodec copy -acodec copy` は再エンコードなし（ロスレス変換）。高速で画質も落ちない。

### 注意点

- DTCP-IP 保護がかかったまま保存した `.m2ts` ファイルは暗号化されており、そのままでは再生できない
- 合法的な手段（DiXiM Play・IO-DATA BDレコ等）でダウンロードしたファイルのみ変換可能
- 日本地上デジタル放送の録画には「コピーワンス」「ダビング10」の制限がある

---

## スマートフォンアプリ

### Video & TV SideView（Sony）

| 項目 | 内容 |
|---|---|
| 開発元 | Sony |
| 対応OS | iOS / Android |
| 対応機器 | BDZ-EW510（確認済み） |
| 機能 | 番組表、録画予約、リモート再生 |
| 価格 | 無料（一部機能は有料） |
| **サービス終了日** | **2027年3月30日** |

**→ BDZ-EW510 をスマホから操作できる唯一の公式アプリだが、サービス終了まで1年強しかない（2026年3月時点）。**

### 注意事項

- リモート再生（外出先から自宅の BDZ にアクセス）には対応しているが、コンテンツをスマホに**ダウンロード保存することはできない**
- 録画番組の「視聴」のみ（コピー不可）

---

## Wireshark による DTCP-IP パケット解析

> ⚠️ これは研究・教育目的の情報。実際の暗号を解読するには DTLA 証明書が必要。

### Wireshark の DTCP-IP ディセクタ

Wireshark には DTCP-IP のパケットを解析するディセクタが組み込まれている:

```
ファイル名: packet-dtcp-ip.c
場所: epan/dissectors/packet-dtcp-ip.c（Wireshark ソース）
```

### 使い方

```
1. Wireshark を起動
2. 対象ネットワークインターフェースでキャプチャ開始
3. BDZ-EW510 と DiXiM Play 等の DTCP-IP 通信をキャプチャ
4. フィルター: tcp.port == 9999 （DTCP-IP デフォルトポート）
   または: dtcp-ip （プロトコルフィルター）
```

### 見えるもの・見えないもの

| 情報 | 可視性 | 理由 |
|---|---|---|
| AKE の通信シーケンス | **見える** | 認証のフロー自体は平文部分あり |
| AKE で使われる Challenge/Response | **見える（暗号化済み）** | 復号には DTLA 証明書が必要 |
| PCP パケット構造 | **見える** | ヘッダー部分（E-EMI 等）は解析可能 |
| コンテンツ（映像・音声データ） | **見えない** | AES-128 で暗号化 |
| コンテンツキー（Kc） | **見えない** | AKE 内で交換、平文では流れない |

### AKE のポート

| ポート | 用途 |
|---|---|
| 9999 (TCP) | DTCP-IP AKE（デフォルト）|
| 動的 | コンテンツ HTTP ストリーム（DLNA で動的割当）|

---

## BDZ-EW510 の DLNA ブラウズ

合法的な調査ツールとして DLNA/UPnP の録画一覧を取得できる（コンテンツの復号は別問題）。

### curl を使った手動 DLNA ブラウズ

```bash
# SSDP でデバイス探索（UDPブロードキャスト）
# ※ 実際には Wireshark や SSDP ライブラリを使う方が容易

# BDZ-EW510 が見つかったら、ContentDirectoryService に SOAP リクエスト
curl -s -X POST \
  -H "Content-Type: text/xml; charset=utf-8" \
  -H "SOAPAction: \"urn:schemas-upnp-org:service:ContentDirectory:1#Browse\"" \
  -d '<?xml version="1.0"?>
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Body>
    <u:Browse xmlns:u="urn:schemas-upnp-org:service:ContentDirectory:1">
      <ObjectID>0</ObjectID>
      <BrowseFlag>BrowseDirectChildren</BrowseFlag>
      <Filter>*</Filter>
      <StartingIndex>0</StartingIndex>
      <RequestedCount>100</RequestedCount>
      <SortCriteria></SortCriteria>
    </u:Browse>
  </s:Body>
</s:Envelope>' \
  http://BDZ-EW510-IP:52235/upnp/control/ContentDirectory1
```

※ ポートと URL パスは機器によって異なる。SSDP レスポンスの `LOCATION` ヘッダーから取得。

---

## チェックリスト（コンテンツ救出前に確認）

- [ ] BDZ-EW510 の動作確認（電源ON・録画一覧表示）
- [ ] HDD の異音・動作異常がないか確認
- [ ] ネットワーク接続確認（BDZ-EW510 と PC が同一 LAN）
- [ ] 利用するソフトの選択（IO-DATA BDレコ / DiXiM Play / SeeQVault）
- [ ] IO-DATA BDレコ互換確認（[互換リスト](https://www.iodata.jp/pio/io/storage/bdrec.htm) 参照）
- [ ] 十分なストレージ（録画 1時間あたり約 15〜20GB）
- [ ] バックアップ先の確保（外付け HDD 推奨）

---

## 参考リンク

- [ffmpeg 公式ドキュメント](https://ffmpeg.org/documentation.html)
- [VLC Media Player](https://www.videolan.org/)
- [Wireshark DTCP-IP ディセクタ（GitHub）](https://github.com/wireshark/wireshark/blob/master/epan/dissectors/packet-dtcp-ip.c)
- [Video & TV SideView（Sony）](https://www.sony.jp/support/software/sideview/)
- [IO-DATA BDレコ 互換リスト](https://www.iodata.jp/pio/io/storage/bdrec.htm)
- [Backblaze HDD 信頼性レポート](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data)

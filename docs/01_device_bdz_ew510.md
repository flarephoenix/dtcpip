# Sony BDZ-EW510 仕様・DLNA 詳細

## 基本仕様

| 項目 | 内容 |
|---|---|
| 発売時期 | 2013年（製造終了済み） |
| HDD 容量 | 500GB |
| チューナー | 地上デジタル×2、BS/110度CS×2 |
| 同時録画 | 2チャンネル |
| Blu-ray | 対応 |
| 無線 LAN | 対応（802.11b/g/n） |
| DLNA | 対応（DMSとして機能） |
| DTCP-IP | 対応 |

## ネットワーク機能

### 対応していること
- DLNA メディアサーバー（DMS）として LAN 上に録画一覧を公開
- DTCP-IP によるコンテンツ保護付き配信
- スマートフォン・タブレットへのリモート視聴（2013年11月ファームウェアアップデートで追加）
- リアルタイムトランスコーディングによるネットワーク配信
- IO-DATA RECBOX への自動ダビング（DTCP-IP 経由）
- PC TV Plus（Sony 公式）との連携

### 対応していないこと
- **ネットワークムーブ（直接）は非対応** ← 重要
  - BD メディアを経由した移動のみ可能
- 他の BDZ への直接ネットワークダビングは限定的

## DLNA コンテンツ構造

BDZ-EW510 が DLNA で公開するコンテンツの構造（標準 UPnP ContentDirectory に準拠）:

```
Root Container
└── 録画番組フォルダ
    ├── 番組1（<res> に DTCP-IP 保護情報付き）
    ├── 番組2
    └── ...
```

### DIDL-Lite レスポンスの例（推定）

```xml
<DIDL-Lite xmlns="urn:schemas-upnp-org:metadata-1-0/DIDL-Lite/"
           xmlns:dc="http://purl.org/dc/elements/1.1/"
           xmlns:upnp="urn:schemas-upnp-org:metadata-1-0/upnp/">
  <item id="1" parentID="0" restricted="1">
    <dc:title>ドキュメント72時間</dc:title>
    <upnp:channelName>NHK総合</upnp:channelName>
    <upnp:scheduledStartTime>2024-03-15T21:00:00</upnp:scheduledStartTime>
    <upnp:scheduledDurationTime>00:45:00</upnp:scheduledDurationTime>
    <upnp:class>object.item.videoItem.videoBroadcast</upnp:class>
    <res protocolInfo="http-get:*:video/mpeg:DLNA.ORG_PN=MPEG_TS_HD_KO_T;DTCP1HOST=192.168.1.10;DTCP1PORT=9999;CONTENTPROTECTION=DTCP"
         size="4294967296">
      http://192.168.1.10:60152/ContentDir/content?id=1
    </res>
  </item>
</DIDL-Lite>
```

### protocolInfo の読み方

`DTCP1HOST=192.168.1.10;DTCP1PORT=9999` の部分が DTCP-IP の AKE サーバー（レコーダー自身）の場所を示している。この情報をもとにクライアントは認証を行う。

## Sony 独自拡張

- **Room Link（ルームリンク）**: Sony の DLNA 拡張で、他の Sony 機器間でのコンテンツ共有を実現
- **x-type-info などの Sony 独自メタデータ**: 番組情報に ARIB データ（放送局コード等）が含まれる場合がある

## 機器固有の注意点

- 2013年モデルのため、現行の DTCP-IP クライアントとの互換性は個別に確認が必要
- ファームウェアは更新終了済みのため、既知のバグ修正は期待できない
- 無線 LAN 接続よりも有線 LAN 接続のほうが転送安定性が高い

## 参考リンク

- [Sony BDZ-EW510 製品ページ（アーカイブ）](https://www.sony.jp/bd/products/BDZ-EW510/)

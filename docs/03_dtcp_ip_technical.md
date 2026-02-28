# DTCP-IP 技術詳細

> 注意: DTCP-IP の完全な仕様は DTLA との契約なしには入手できない。
> 本ドキュメントは公開情報・RFC・CableLabs オープンソースプロジェクトの情報に基づく。

## AKE（Authentication and Key Exchange）

### 概要

クライアント（Sink）とレコーダー（Source）が、コンテンツ配信前に行う認証・鍵交換プロセス。

### ステップ

```
1. 証明書交換
   Client → Server: DTLA 発行の Sink 証明書
   Client ← Server: DTLA 発行の Source 証明書

2. 相互認証
   - EC-DSA（楕円曲線デジタル署名）で証明書の正当性を検証
   - DTLA の CA（認証局）による署名を確認

3. 鍵交換
   - EC-DH（楕円曲線ディフィー・ヘルマン）で共有秘密を生成
   - この共有秘密からセッション鍵（Kx）を導出

4. 認証完了
   - セッションハンドル（整数）が払い出される
   - 以降のコンテンツ復号にこのハンドルを使用
```

### なぜ自前実装が不可能か

- **DTLA 証明書が必要**: 証明書は DTLA ライセンス契約者にのみ発行される
- **楕円曲線パラメータが非公開**: DTLA 独自のカーブパラメータを使用
- 証明書なしでは Source 側がアクセスを拒否する

## PCP（Protected Content Packet）パケット構造

DTCP-IP コンテンツは HTTP レスポンスボディで PCP 形式にパッキングされて届く。

```
┌─────────────────────────────────────────┐
│ PCP ヘッダー（4 bytes）                  │
│  Bit 31    : E-EMI（1=暗号化あり）       │
│  Bits 30-28: E-EMI 値（コピー制御）      │
│  Bit 27    : C_A（コンテンツタイプ）     │
│  Bits 26-0 : ペイロードサイズ（16B アライン）│
├─────────────────────────────────────────┤
│ Nonce（16 bytes）                        │
│  ※ シーケンス先頭パケットのみ           │
├─────────────────────────────────────────┤
│ 暗号化ペイロード（AES-128-CBC）          │
│  ※ ヘッダーで示されたサイズ分           │
└─────────────────────────────────────────┘
```

### 処理フロー

1. HTTP レスポンスストリームから 4 バイト読む（PCP ヘッダー）
2. ペイロードサイズを計算（`bits[26:0]`）
3. 必要なら Nonce（16B）を読む
4. ペイロードを読む
5. DTCP-IP ライブラリの復号関数に渡す → 平文が返る
6. 1 に戻る

## DLNA HTTP ヘッダー

DTCP-IP 対応コンテンツの HTTP リクエストに必要なヘッダー:

```http
GET /ContentDir/content?id=1 HTTP/1.1
Host: 192.168.1.10:60152
transferMode.dlna.org: Bulk
getcontentFeatures.dlna.org: 1
Connection: keep-alive
```

### protocolInfo の読み方

DIDL-Lite の `<res>` 要素に含まれる `protocolInfo` 属性:

```
http-get:*:video/mpeg:DLNA.ORG_PN=MPEG_TS_HD_KO_T;DTCP1HOST=192.168.1.10;DTCP1PORT=9999;CONTENTPROTECTION=DTCP
```

| フィールド | 値 | 意味 |
|---|---|---|
| `DLNA.ORG_PN` | `MPEG_TS_HD_KO_T` | HD MPEG-2 TS（日本向け） |
| `DTCP1HOST` | `192.168.1.10` | AKE サーバー（レコーダー）の IP |
| `DTCP1PORT` | `9999` | AKE の TCP ポート番号 |
| `CONTENTPROTECTION` | `DTCP` | DTCP-IP 保護されていることを示す |

## 出力コンテンツ形式

BDZ-EW510 が配信するコンテンツ:

| 形式 | 説明 |
|---|---|
| **BDAV MPEG-2 TS（M2TS）** | 192 バイト/パケット（4 バイトのタイムスタンプ付き） |
| コーデック | MPEG-2 Video（HD 録画）または H.264（スマホ向け低解像度） |
| 音声 | AAC または MPEG-2 Audio |

復号後のファイルは `.m2ts` として保存すると VLC / mpv / MPC-HC で再生可能。

## CableLabs RDK の C API（公開情報）

[gst-plugins-dtcpip](https://github.com/lyrato/gst-plugins-dtcpip) が参照する CableLabs 定義の API:

```c
// 初期化
int dtcpip_cmn_init(const char *storage_path);
int dtcpip_snk_init(void);

// AKE（認証・鍵交換）
int dtcpip_snk_open(const char *ip_addr, unsigned short port, int *session_handle);

// コンテンツ復号
int dtcpip_snk_alloc_decrypt(
    int session_handle,
    unsigned char *encrypted_data, size_t encrypted_size,
    unsigned char **cleartext_data, size_t *cleartext_size);
int dtcpip_snk_free(unsigned char *cleartext_data);

// セッション終了
int dtcpip_snk_close(int session_handle);
```

この API は CableLabs の RDK（Reference Design Kit）プロジェクトで定義されており、GStreamer プラグインはこの関数群を持つ DLL をランタイムでロードする設計になっている。

## 参考リンク

- [lyrato/gst-plugins-dtcpip](https://github.com/lyrato/gst-plugins-dtcpip) - C API の定義が確認できる
- [DTCP-IP informational spec（DTLA 公開版・実装詳細は省略）](https://www.dtcp.com/specifications.aspx)
- [RFC 7562 - TLS Authorization Using DTCP Certificates](https://www.rfc-editor.org/rfc/rfc7562)

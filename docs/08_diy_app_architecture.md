# 自作アプリのアーキテクチャ（参考）

> ⚠️ 免責事項
>
> 本ドキュメントは教育・研究目的の記録である。
> DTCP-IP の実装には DTLA ライセンスが必要。
> DTLA ライセンスなしに DTCP-IP 保護コンテンツを復号することは、
> 日本の著作権法上グレーゾーン〜違法となる可能性がある。
> 詳細は [05_japanese_copyright_law.md](./05_japanese_copyright_law.md) を参照。

## 諦めた理由の整理

| 問題 | 詳細 |
|---|---|
| DTCP-IP ライブラリが非公開 | AKE に必要な DTLA 証明書・ライブラリは DTLA 契約者のみ入手可 |
| OSS 実装が存在しない | GStreamer プラグインは「殻」のみ。コアの DLL は非公開 |
| ライセンス費用が現実的でない | 数十万〜数百万円/年規模と推測。個人利用では不合理 |
| 法的グレーゾーン | 個人利用でも著作権法上の「技術的保護手段の回避」に該当する可能性 |

## もし作るとしたら（アーキテクチャ設計）

### 技術スタック

| 選択肢 | 理由 |
|---|---|
| **C# .NET 8 + WPF**（推奨） | Windows ネイティブ、P/Invoke による DLL 呼び出しが容易、エコシステムが充実 |
| Rust + Tauri | クロスプラットフォーム、バイナリが小さい、ただし DLL interop が複雑 |
| Electron / Node.js | クロスプラットフォーム、ただし配布サイズが大きい |

### プロジェクト構成

```
DtcpipDownloader.sln
├── src/
│   ├── DtcpipDownloader.App/        (WPF, エントリーポイント)
│   │   ├── Views/
│   │   │   ├── DeviceListView.xaml  (レコーダー探索・選択)
│   │   │   ├── RecordingListView.xaml (録画番組一覧)
│   │   │   └── DownloadView.xaml    (ダウンロードキュー)
│   │   └── ViewModels/              (CommunityToolkit.Mvvm 使用)
│   │
│   └── DtcpipDownloader.Core/       (ロジック層、WPF 非依存)
│       ├── Upnp/
│       │   ├── SsdpDiscoveryService    (Rssdp NuGet)
│       │   ├── ContentDirectoryClient  (SOAP で CDS Browse)
│       │   ├── SoapClient              (HttpClient + SOAP エンベロープ)
│       │   └── DidlParser              (DIDL-Lite XML → 録画メタデータ)
│       ├── Dtcpip/
│       │   ├── IDtcpipProvider         (抽象インターフェース)
│       │   ├── DllDtcpipProvider       (外部 DLL を動的ロード)
│       │   ├── NullDtcpipProvider      (非保護コンテンツ用)
│       │   └── DtcpipStream            (PCP パケットを復号する Stream)
│       └── Download/
│           ├── DownloadManager         (AKE → HTTP → 復号 → 保存)
│           └── HttpDlnaClient          (DLNA 必須ヘッダー付き HTTP)
└── tests/
    └── DtcpipDownloader.Tests/
```

### DTCP-IP DLL のロード方法（.NET 8）

```csharp
// .NET 8 クロスプラットフォーム対応のネイティブ DLL ロード
// Windows: LoadLibrary 相当、Mac: dlopen 相当を自動選択
var handle = NativeLibrary.Load(dllPath);
var fnPtr  = NativeLibrary.GetExport(handle, "dtcpip_cmn_init");
var fn     = Marshal.GetDelegateForFunctionPointer<DtcpipCmnInit>(fnPtr);
```

### DLL が期待する C API（CableLabs RDK 定義・公開情報）

参照: [lyrato/gst-plugins-dtcpip の rui_dtcpip.h](https://github.com/lyrato/gst-plugins-dtcpip)

```c
int dtcpip_cmn_init(const char *storage_path);  // 初期化
int dtcpip_snk_init(void);                       // Sink 初期化

// AKE（認証・鍵交換）
int dtcpip_snk_open(const char *ip, unsigned short port, int *handle);

// PCP コンテンツ復号
int dtcpip_snk_alloc_decrypt(
    int handle,
    unsigned char *enc, size_t enc_len,
    unsigned char **dec, size_t *dec_len);
int dtcpip_snk_free(unsigned char *dec);

// 終了
int dtcpip_snk_close(int handle);
```

### DTCP-IP DLL の入手候補パス（グレーゾーン）

> ⚠️ 以下はライセンス上グレーな手法。ソフトウェアライセンス違反の可能性あり。

DTLA ライセンス済みのソフトをインストールすると DLL が含まれる可能性がある:

```
# Windows
C:\Program Files (x86)\DigiOn\DiXiM Digital TV plus\dtcpip.dll
C:\Program Files (x86)\Sony\PC TV Plus\dtcpip.dll
C:\Program Files\CyberLink\PowerDVD\dtcpip.dll
```

DLL が見つかれば、自作アプリがその DLL を `NativeLibrary.Load()` でロードして AKE を実行できる可能性がある。ただし DLL の実際のエクスポート関数名はベンダー依存で、上記の関数名と一致しない場合がある。

### Mac 対応のために必要な変更

| 変更箇所 | Windows | Mac |
|---|---|---|
| UI | WPF | Avalonia UI（XAML 互換）に差し替え |
| DLL ロード | （同じ NativeLibrary.Load で OK） | Mac 版の DTCP-IP DLL が必要 |
| 設定保存先 | `%APPDATA%` | `~/Library/Application Support/` |
| ビルド | `-r win-x64` | `-r osx-arm64` |

### 実装ステップ

1. **Week 1**: UPnP/SSDP 探索 → BDZ-EW510 を LAN 上で発見
2. **Week 2**: DLNA CDS Browse → 録画一覧取得・DIDL-Lite パース
3. **Week 3**: HTTP ダウンロード → 非保護コンテンツで動作確認
4. **Week 4**: DTCP-IP DLL 統合 → AKE → 復号 → ファイル保存 ← **ここが最大の壁**
5. **Week 5**: エラーハンドリング・設定 UI・単体ファイル配布

### 出力形式

- **`.m2ts`（BDAV MPEG-2 TS）**: 192 バイト/パケット。VLC / mpv / MPC-HC で再生可能
- ファイル名例: `NHK総合_20240315_2100_ドキュメント72時間.m2ts`

## 参考リンク

- [lyrato/gst-plugins-dtcpip](https://github.com/lyrato/gst-plugins-dtcpip) - CableLabs C API 定義
- [Rssdp（UPnP/SSDP ライブラリ for .NET）](https://github.com/Yortw/RSSDP)
- [CommunityToolkit.Mvvm](https://learn.microsoft.com/ja-jp/dotnet/communitytoolkit/mvvm/)
- [NativeLibrary クラス（.NET 8）](https://learn.microsoft.com/ja-jp/dotnet/api/system.runtime.interopservices.nativelibrary)

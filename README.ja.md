# Mock-WebBluetooth (日本語)

🇯🇵 [日本語](README.ja.md) | 🇺🇸 [English](README.en.md) | 🇨🇳 [简体中文](README.zh.md)

## 概要

Web Bluetooth API の互換性を目的とした拡張型互換性APIです。

特定ブラウザの内部実装をそのまま移植するのではなく、Web Bluetooth API の互換性を維持しながら、実行環境に合わせた独自実装・拡張を可能にすることを目的としています。

## 🌐 実装

### pyside6-webbluetooth

PySide6 / QtWebEngine アプリケーション向けの Web Bluetooth API 実装です。

- `navigator.bluetooth` を提供
- BLE デバイスとの実機通信
- ネイティブデバイス選択ダイアログ
- ライブスキャン
- Origin ごとのデバイス権限
- フレーム単位の Origin 検証
- GATT Service / Characteristic / Descriptor へのアクセス
- GATT Blocklist
- `requestDevice()` の主要なフィルタ処理
- Qt UI をブロックしない非同期 BLE Worker
- QtWebEngine / QWebChannel ブリッジ

Repository: https://github.com/steck0714/pyside6-webbluetooth

## 設計

Web Bluetooth では、BLE のスキャン、接続、GATT 探索、read/write/notify など、比較的時間のかかる非同期処理が発生します。

そのため、QtWebEngine の UI スレッドを長時間ブロックしない構成を採用しています。

```text
Webページ
   │
   │ navigator.bluetooth
   ▼
Web Bluetooth 互換レイヤー
   │
   │ QWebChannel
   ▼
Bluetooth Bridge
   │
   ├── Origin / Frame 検証
   ├── 権限管理
   ├── デバイス選択
   ├── GATT Blocklist
   └── API / 引数検証
   │
   ▼
BLE Worker / asyncio
   │
   │ bleak
   ▼
BLEデバイス
```

## 互換性

Chrome / Chromium の Web Bluetooth の挙動を互換性の参考にしていますが、本プロジェクトは Chrome の内部実装そのものではありません。

> Web Bluetooth互換 ≠ Chromeクローン

API の互換性を重視しながら、Python、PySide6、QtWebEngine、BLE バックエンドなどの実行環境に合わせた独立実装を想定しています。

## セキュリティ

Origin ごとの権限管理、フレーム単位の Origin 検証など、Web Bluetooth のセキュリティモデルを意識した設計になっています。

GATT Blocklist によって、保護対象となる Service / Characteristic / Descriptor へのアクセスを制限します。

## ステータス

⚠️ 初期段階のプロジェクトです。

実際の利用環境、ブラウザ互換性、BLE デバイスとの組み合わせによって挙動が異なる可能性があります。

特にハードウェア制御やセキュリティ境界として利用する場合は、必ずコードと実行環境を確認してください。

## License

MIT License

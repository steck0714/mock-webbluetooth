# Mock-WebBluetooth (English)

🇯🇵 [日本語](README.ja.md) | 🇺🇸 [English](README.en.md) | 🇨🇳 [简体中文](README.zh.md)

## About

An extended compatibility API focused on compatibility with the Web Bluetooth API.

The project is not intended to directly port the internal implementation of a specific browser. Instead, it aims to preserve Web Bluetooth API compatibility while allowing independent implementations and extensions for different runtime environments.

## 🌐 Implementation

### [pyside6-webbluetooth](https://github.com/steck0714/pyside6-webbluetooth)

A Web Bluetooth API implementation for PySide6 / QtWebEngine applications.

- Provides `navigator.bluetooth`
- Real BLE device communication
- Native device selection dialog
- Live scanning
- Per-Origin device permissions
- Frame-level Origin verification
- Access to GATT Service / Characteristic / Descriptor
- GATT Blocklist
- Main filtering support for `requestDevice()`
- Asynchronous BLE Worker that does not block the Qt UI
- QtWebEngine / QWebChannel bridge

## Design

Web Bluetooth involves asynchronous operations such as BLE scanning, connection, GATT discovery, read/write, and notifications. These operations can take significantly longer than many ordinary UI operations.

The implementation therefore aims to avoid blocking the QtWebEngine UI thread for long periods.

```text
Web page
   │
   │ navigator.bluetooth
   ▼
Web Bluetooth compatibility layer
   │
   │ QWebChannel
   ▼
Bluetooth Bridge
   │
   ├── Origin / Frame validation
   ├── Permission management
   ├── Device selection
   ├── GATT Blocklist
   └── API / argument validation
   │
   ▼
BLE Worker / asyncio
   │
   │ bleak
   ▼
BLE device
```

## Compatibility

Chrome / Chromium Web Bluetooth behavior is used as a compatibility reference, but this project is not a direct port of Chrome's internal implementation.

> Web Bluetooth compatibility ≠ Chrome clone

The focus is API compatibility while allowing an independent implementation adapted to Python, PySide6, QtWebEngine, and the underlying BLE backend.

## Security

The design takes the Web Bluetooth security model into account, including per-Origin permission management and per-frame Origin validation.

A GATT Blocklist is used to restrict access to protected Services, Characteristics, and Descriptors.

## Status

⚠️ Early-stage project.

Behavior may vary depending on the runtime environment, browser compatibility layer, and BLE device being used. Always review the source code and runtime environment before using it as a hardware-control component or security boundary.

## License

MIT License
